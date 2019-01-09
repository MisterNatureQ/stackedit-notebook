# [[Golang] protoactor-go 201: How middleware works to intercept incoming and outgoing messages](https://blog.oklahome.net/2018/11/protoactor-go-middleware.html) 中间件如何拦截传入和传出的消息

As described in a previous article,  [Protoactor-go 101: How actors communicate with each other](https://blog.oklahome.net/2018/09/protoactor-go-messaging-protocol.html), the core of actor system is message passing. Fine-grained actors work on their own tasks, communicate with each other by passing messages and achieve a bigger task as a whole. To intercept the incoming and outgoing messages to execute tasks before and after the message handling, protoactor supports middleware mechanism.  
Protoactor’s plugin mechanism is built on top of this middleware mechanism so knowing middleware is vital to building highly customized actor.

如前一篇文章[Protoactor-go 101：演员如何相互沟通](https://blog.oklahome.net/2018/09/protoactor-go-messaging-protocol.html)所述，actor 系统的核心是消息传递。  细粒度的参与者可以完成自己的任务，通过传递信息来相互沟通，从而实现更大的任务。  为了拦截传入和传出的消息以在消息处理之前和之后执行任务，protoactor支持中间件机制。  
Protoactor的插件机制建立在这个中间件机制之上，因此了解中间件对于构建高度自定义的actor至关重要。

-   [Types of middleware](https://blog.oklahome.net/2018/11/protoactor-go-middleware.html#toc_0)
-   [Under the hood](https://blog.oklahome.net/2018/11/protoactor-go-middleware.html#toc_1)
-   [Example](https://blog.oklahome.net/2018/11/protoactor-go-middleware.html#toc_2)
-   [Conclusion](https://blog.oklahome.net/2018/11/protoactor-go-middleware.html#toc_3)

# Types of middleware

To intercept incoming and outgoing messages, two kinds of middleware are provided:  [`actor.InboundMiddleware`](https://github.com/AsynkronIT/protoactor-go/blob/f373762276d85e13d8b2999e58e36d01d44e569d/actor/props.go#L5)  and  [`actor.OutboundMiddleware`](https://github.com/AsynkronIT/protoactor-go/blob/f373762276d85e13d8b2999e58e36d01d44e569d/actor/props.go#L6). Inbound middleware is invoked when a message reaches an actor; Outbound middleware is invoked when a message is sent to another actor. Multiple middlewares can be registered for a given actor so it is possible to divide middleware’s tasks into small pieces and create a middleware for each of them in favor of maintainability.

# Under the hood

To register a middleware to an actor, use  `Props.WithMiddleware()`  or  `Props.WithOutboundMiddleware()`. Passed middleware implementations are appended to an internal slice so they can be referenced on actor construction.

```go
// Assign one or more middlewares to the props
func (props *Props) WithMiddleware(middleware ...InboundMiddleware) *Props {
   props.inboundMiddleware = append(props.inboundMiddleware, middleware...)
   return props
}
  
func (props *Props) WithOutboundMiddleware(middleware ...OutboundMiddleware) *Props {
   props.outboundMiddleware = append(props.outboundMiddleware, middleware...)
   return props
}
```

On actor construction, stashed middlewares are transformed into a middleware chain. At this point a group of one or more middlewares are combined together and shape one  `actor.ActorFunc()`. Middlewares are processed in reversed order in this process so they are executed in the registered order on message reception.

```go
func makeInboundMiddlewareChain(middleware []InboundMiddleware, lastReceiver ActorFunc) ActorFunc {  
   if len(middleware) == 0 {  
      return nil  
   }  
  
   h := middleware[len(middleware)-1](lastReceiver)  
   for i := len(middleware) - 2; i >= 0; i-- {  
      h = middleware[i](h)  
   }  
   return h  
}  
  
func makeOutboundMiddlewareChain(outboundMiddleware []OutboundMiddleware, lastSender SenderFunc) SenderFunc {  
   if len(outboundMiddleware) == 0 {  
      return nil  
   }  
  
   h := outboundMiddleware[len(outboundMiddleware)-1](lastSender)  
   for i := len(outboundMiddleware) - 2; i >= 0; i-- {  
      h = outboundMiddleware[i](h)  
   }  
   return h  
}
```

When  `actor.Context`  handles an incoming message, the  `actor.Context`  that holds all the contextual information including message itself is passed to that middleware chain. One important thing to notice at this point is that the original message reception method,  `actor.Receive()`, is wrapped in an anonymous function to match  `actor.ActorFunc()`  signature and is registered to the very end of the middleware chain. So when the context information is passed to the middleware chain, middlewares are executed in the registered order and  `actor.Receive()`  is called at last.

```go
func (ctx *localContext) processMessage(m interface{}) {  
   ctx.message = m  
  
   if ctx.inboundMiddleware != nil {  
      ctx.inboundMiddleware(ctx)  
   } else {  
      if _, ok := m.(*PoisonPill); ok {  
         ctx.self.Stop()  
      } else {  
         ctx.receive(ctx)  
      }  
   }  
  
   ctx.message = nil  
}
```

Likewise, when a message is being sent to another actor, the registered outbound middlewares are executed in the registered order.

# Example

Below is an example that leaves log messages around  `Actor.Receive()`  invocation and  `Context.Request()`.

Below is an example that leaves log messages when message a comes and goes. As the comment suggests, inbound middleware can run its task before and/or after  `actor.Receive()`  execution. Similarly, outbound middleware can run a task before and/or after the original message sending logic. The event occurrence order is described in the comment section of the example.

package main

import (

"github.com/AsynkronIT/protoactor-go/actor"

"log"

"os"

"os/signal"

"syscall"

"time"

)

func newInboundMiddleware() actor.InboundMiddleware {

cnt := 0

return func(next actor.ActorFunc) actor.ActorFunc {

return func(ctx actor.Context) {

cnt++

log.Printf("Start handling incoming message #%d: %#v", cnt, ctx.Message())

next(ctx)

log.Printf("End handling incoming message #%d: %#v", cnt, ctx.Message())

}

}

}

func newOutboundMiddleware() actor.OutboundMiddleware {

cnt := 0

return func(next actor.SenderFunc) actor.SenderFunc {

return func(ctx actor.Context, target *actor.PID, envelope *actor.MessageEnvelope) {

cnt++

log.Printf("Start sending message #%d to %s", cnt, target.Id)

next(ctx, target, envelope)

log.Printf("End sending message #%d to %s", cnt, target.Id)

}

}

}

type ping struct{}

type pong struct{}

func main() {

pongProps := actor.

FromFunc(func(ctx actor.Context) {

switch ctx.Message().(type) {

case *ping:

ctx.Respond(&pong{})

}

})

pongPid, _ := actor.SpawnNamed(pongProps, "pong")

// Output should be somewhat like below.

// Because ping actor receives both signal of struct{}{} and a pong message of &pong{},

// the printed number of execution is doubled comparing to that of pong actor.

//

// 2018/11/24 12:59:25 Start handling incoming message #1: &actor.Started{}

// 2018/11/24 12:59:25 End handling incoming message #1: &actor.Started{}

// 2018/11/24 12:59:26 Start handling incoming message #2: struct {}{}

// 2018/11/24 12:59:26 Received signal

// 2018/11/24 12:59:26 Start sending message #1 to pong

// 2018/11/24 12:59:26 End sending message #1 to pong

// 2018/11/24 12:59:26 End handling incoming message #2: struct {}{}

// 2018/11/24 12:59:26 Start handling incoming message #3: &main.pong{}

// 2018/11/24 12:59:26 Received pong

// 2018/11/24 12:59:26 End handling incoming message #3: &main.pong{}

// 2018/11/24 12:59:27 Start handling incoming message #4: struct {}{}

// 2018/11/24 12:59:27 Received signal

// 2018/11/24 12:59:27 Start sending message #2 to pong

// 2018/11/24 12:59:27 End sending message #2 to pong

// 2018/11/24 12:59:27 End handling incoming message #4: struct {}{}

// 2018/11/24 12:59:27 Start handling incoming message #5: &main.pong{}

// 2018/11/24 12:59:27 Received pong

// 2018/11/24 12:59:27 End handling incoming message #5: &main.pong{}

// 2018/11/24 12:59:28 Start handling incoming message #6: struct {}{}

// 2018/11/24 12:59:28 Received signal

// 2018/11/24 12:59:28 Start sending message #3 to pong

// 2018/11/24 12:59:28 End sending message #3 to pong

// 2018/11/24 12:59:28 End handling incoming message #6: struct {}{}

// 2018/11/24 12:59:28 Start handling incoming message #7: &main.pong{}

// 2018/11/24 12:59:28 Received pong

// 2018/11/24 12:59:28 End handling incoming message #7: &main.pong{}

// ^C2018/11/24 12:59:29 Finish

pingProps := actor.

FromFunc(func(ctx actor.Context) {

switch ctx.Message().(type) {

case struct{}:

log.Print("Received signal")

ctx.Request(pongPid, &ping{})

case *pong:

log.Print("Received pong")

}

}).

WithMiddleware(newInboundMiddleware()).

WithOutboundMiddleware(newOutboundMiddleware())

pingPid, _ := actor.SpawnNamed(pingProps, "ping")

finish := make(chan os.Signal, 1)

signal.Notify(finish, os.Interrupt)

signal.Notify(finish, syscall.SIGTERM)

ticker := time.NewTicker(1 * time.Second)

defer ticker.Stop()

for {

select {

case <-ticker.C:

pingPid.Tell(struct{}{})

case <-finish:

pingPid.Stop()

pongPid.Stop()

log.Print("Finish")

return

}

}

}

[view raw](https://gist.github.com/oklahomer/d6fb6337d5aa5915e9457c77fbe1b0a9/raw/5cf7a10b8f3d929ad42127eb45e5379fd8126ce5/middleware.go)[middleware.go](https://gist.github.com/oklahomer/d6fb6337d5aa5915e9457c77fbe1b0a9#file-middleware-go)  hosted with ❤ by  [GitHub](https://github.com/)

# Conclusion

Middleware mechanism can be used to run a certain logic before and after the original method invocation in an AOP-ish manner.




<!--stackedit_data:
eyJoaXN0b3J5IjpbLTExMTc3OTU5MzcsODU1NDQ2MzRdfQ==
-->