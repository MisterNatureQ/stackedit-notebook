# [[Golang] protoactor-go 101: How actor.Future works to synchronize concurrent task execution](https://blog.oklahome.net/2018/11/protoactor-go-how-future-works.html)  actor.Future如何同步并发任务执行

Fine-grained actors concentrate on their own tasks and communicate with others to accomplish a bigger task as a whole. That is how a well-designed actor system works. Because each actor handles a smaller part of an incoming task, pass it to another and then proceed to work on the next incoming task, actors can efficiently work in a concurrent manner to accomplish more tasks in the same amount of time. To pass a result of one actor’s job execution to another or to execute a task when another actor’s execution is done, actor.Future mechanism comes in handy.

细粒度的actors 专注于自己的任务，并与他人沟通，以完成一个更大的任务。  这就是精心设计的 actor 系统的运作方式。  因为每个actor处理传入任务的较小部分，将其传递给另一个，然后继续处理下一个传入任务，actor可以以并发方式有效地工作，以在相同的时间内完成更多任务。**  要将一个actor的作业执行结果传递给另一个actor，或者在另一个actor的执行完成时执行任务，actor.Future机制就派上用场了。** 

-   [Introducing Future](https://blog.oklahome.net/2018/11/protoactor-go-how-future-works.html#toc_0)
-   [How this works under the hood](https://blog.oklahome.net/2018/11/protoactor-go-how-future-works.html#toc_1)
-   [Future.Wait() / Future.Result()](https://blog.oklahome.net/2018/11/protoactor-go-how-future-works.html#toc_2)
-   [Future.PipeTo()](https://blog.oklahome.net/2018/11/protoactor-go-how-future-works.html#toc_3)
-   [Context.AwaitFuture()](https://blog.oklahome.net/2018/11/protoactor-go-how-future-works.html#toc_4)
-   [Conclusion](https://blog.oklahome.net/2018/11/protoactor-go-how-future-works.html#toc_5)

# Introducing Future 介绍 Future 

The basic idea of “future” in this context is quite similar to that of  `future`  and  `promise`  implemented by many modern programming languages to coordinate concurrent executions.
“future”的基本思想非常类似于`future`和`promise` 许多现代编程语言为 协调并发执行 的实现。

For example,  [`Future`](https://docs.oracle.com/javase/8/docs/api/index.html?java/util/concurrent/Future.html)’s Javadoc reads as below:

> A`Future`represents the result of an asynchronous computation. Methods are provided to check if the computation is complete, to wait for its completion, and to retrieve the result of the computation.
>`Future`表示异步计算的结果。  提供方法以检查计算是否完成，等待其完成，以及取回计算结果。

In protoactor-go,  `actor.Future`  provides methods to wait for destination actor’s response in a blocking manner, to pipe one actor’s response to another in a non-blocking manner and to execute a callback function when destination actor’s response arrives.
在protoactor-go中， `actor.Future`提供了以阻塞方式等待目标actor的响应的方法，以非阻塞方式将一个actor的回应 传递给另一个actor，并在目标actor的 回应 到达时执行回调函数。

## How this works under the hood

The implementation of protoactor-go’s Future mechanism is composed of  [`actor.Future`](https://github.com/AsynkronIT/protoactor-go/blob/f373762276d85e13d8b2999e58e36d01d44e569d/actor/future.go#L35-L45)  and  [`actor.futureProcess`](https://github.com/AsynkronIT/protoactor-go/blob/f373762276d85e13d8b2999e58e36d01d44e569d/actor/future.go#L109-L112), where  `actor.Future`  provides common Future methods while  `actor.futureProcess`  wraps  `actor.Future`  and works as a  `actor.Process`. A developer may call  `Context.RequestFuture()`  or  `PID.RequestFuture()`  instead of commonly used  `Context.Request()`or  `PID.Request()`  to acquire  `actor.Future`  that represents a non-determined result.

protoactor-go的Future机制的实现由[`actor.Future`](https://github.com/AsynkronIT/protoactor-go/blob/f373762276d85e13d8b2999e58e36d01d44e569d/actor/future.go#L35-L45)和[`actor.futureProcess`](https://github.com/AsynkronIT/protoactor-go/blob/f373762276d85e13d8b2999e58e36d01d44e569d/actor/future.go#L109-L112)组成，其中`actor.Future`提供常见的Future方法，而`actor.futureProcess`包含`actor.Future`并作为`actor.Process`  。  开发人员可以调用`Context.RequestFuture()`或`PID.RequestFuture()`而不是常用的`Context.Request()`或`PID.Request()`来获取表示未确定结果的`actor.Future`  。

In those method calls,  `actor.NewFuture()`  is called with preferred timeout duration as an argument. In  `actor.NewFuture()`,  `actor.Future`  and its wrapper –  `actor.futureProcess`  – are both constructed,  `actor.futureProcess`  is registered to protoactor-go’s internal process registry for a later reference and  `actor.Future`  is returned.

在这些方法调用中，调用`actor.NewFuture()`  ，  `actor.NewFuture()`首选超时持续时间作为参数。  在`actor.NewFuture()`  ，  `actor.Future`及其包装器 -  `actor.futureProcess`  - 都被构造，  `actor.futureProcess`被注册到protoactor-go的内部进程注册表以供稍后的引用和`actor.Future`返回。

As depicted in the below code fragment,  `actor.Future`’s  `actor.PID`  is set as a  `Sender`  of the requesting message. So when the receiving actor responds to the sender, the response is actually sent to the  `actor.Future`’s  `actor.PID`. When  `actor.Future`  receives the response, the result of the Future is set and becomes available to the subscriber.
如下面的代码片段所示，  `actor.Future`的`actor.PID`被设置为请求消息的发送者。  因此，当`actor.Future`响应发送者时，响应实际上被发送给了`actor.Future`的`actor.PID`  。当`actor.Future`收到响应时，将设置Future的结果并可以订阅使用。

```go
func (ctx *localContext) RequestFuture(pid *PID, message interface{}, timeout time.Duration) *Future {  
   future := NewFuture(timeout)  
   env := &MessageEnvelope{  
      Header:  nil,  
  Message: message,  
  // `actor.Future`的`actor.PID`被设置为请求消息的发送者
  Sender:  future.PID(),  
  }  
   ctx.sendUserMessage(pid, env)  
  
   return future  
}
```

The usage of the  `actor.Future`  returned by those methods are covered in later sections with detailed example codes. All example codes are available at  [github.com/oklahomer/protoactor-go-future-example](https://github.com/oklahomer/protoactor-go-future-example).

这些方法返回的`actor.Future`的用法将在后面的章节中详细介绍示例代码。  所有示例代码均可在[github.com/oklahomer/protoactor-go-future-example上获得](https://github.com/oklahomer/protoactor-go-future-example).

# Future.Wait() / Future.Result()

To wait until the execution times out or the destination actor responds, use  `Future.Wait()`  or  `Future.Result()`. They both internally call a blocking private method,  `Future.wait()`, to block till the preconfigured timeout duration passes or the execution completes. The only difference is whether to return the result of the computation;  `Future.Wait()`  simply waits for completion just like  [`WaitGroup.Wait()`](https://golang.org/pkg/sync/#WaitGroup.Wait)  while  `Future.Result()`  waits for completion and additionally returns the result ot the computation as its name suggests.

要等到执行超时或目标actor响应，请使用`Future.Wait()`或`Future.Result()`  。  它们都在内部调用阻塞私有方法`Future.wait()`  ，直到预配置的超时持续时间超过或执行完成为止。  唯一的区别是是否返回计算结果;  `Future.Wait()`只是像[`WaitGroup.Wait()`](https://golang.org/pkg/sync/#WaitGroup.Wait)一样等待完成，而`Future.Result()`等待完成，另外返回计算结果，顾名思义。

![](https://2.bp.blogspot.com/-1UlqPWNdFaY/W_fgLwxkgnI/AAAAAAAAA4Y/TwcDNHeR9Gg0QPALyRoHPvkVuh4udkysgCPcBGAYYCw/s1600/timeline.png)

```go

package main

import (
"github.com/AsynkronIT/protoactor-go/actor"
"log"
"os"
"os/signal"
"syscall"
"time"
)

type pong struct {

}

type ping struct {

}

type pongActor struct {
	timeOut bool
}

func (p *pongActor) Receive(ctx actor.Context) {
	// Dead letter occurs because the PID of Future process ends and goes away when Future times out
	// so the pongActor fails to send message.
	switch ctx.Message().(type) {
		case *ping:
			var sleep time.Duration
			if p.timeOut {
				sleep = 2500 * time.Millisecond
				p.timeOut = false
			} else {
				sleep = 300 * time.Millisecond
				p.timeOut = true
			}
			time.Sleep(sleep)
			ctx.Sender().Tell(&pong{})
	}
}

type pingActor struct {
	pongPid *actor.PID
}

func (p *pingActor) Receive(ctx actor.Context) {

	switch ctx.Message().(type) {
		case struct{}:
		// Output becomes somewhat like below.
		// See a diagram at https://raw.githubusercontent.com/oklahomer/protoactor-go-future-example/master/docs/wait/timeline.png
		//
		// 2018/10/13 17:03:22 Received pong message &main.pong{}
		// 2018/10/13 17:03:24 Timed out
		// 2018/10/13 08:03:26 [ACTOR] [DeadLetter] pid="nonhost/future$4" message=&{} sender="nil"
		// 2018/10/13 17:03:26 Received pong message &main.pong{}
		// 2018/10/13 17:03:28 Timed out
		// 2018/10/13 08:03:30 [ACTOR] [DeadLetter] pid="nonhost/future$6" message=&{} sender="nil"
		// 2018/10/13 17:03:30 Received pong message &main.pong{}
		future := p.pongPid.RequestFuture(&ping{}, 1*time.Second)

		// Future.Result internally waits until response comes or times out
		result, err := future.Result()

		if err != nil {
			log.Print("Timed out")
			return
		}
		log.Printf("Received pong message %#v", result)
	}
}

func main() {
	pongProps := actor.FromProducer(func() actor.Actor {
		return &pongActor{}
	})

	pongPid := actor.Spawn(pongProps)
	pingProps := actor.FromProducer(func() actor.Actor {
		return &pingActor{
			pongPid: pongPid,
		}
	})

	pingPid := actor.Spawn(pingProps)
	finish := make(chan os.Signal, 1)
	signal.Notify(finish, os.Interrupt)
	signal.Notify(finish, syscall.SIGTERM)
	ticker := time.NewTicker(2 * time.Second)
	defer ticker.Stop()

	for {
		select {
			case <-ticker.C:
				pingPid.Tell(struct{}{})
			case <-finish:
				return
			log.Print("Finish")
		}
	}
}
```
[view raw](https://gist.github.com/oklahomer/96001c239d871890087e673d5908c3a9/raw/236c400f2004955211ccbc9dbf27b65c6096a264/wait.go)[wait.go](https://gist.github.com/oklahomer/96001c239d871890087e673d5908c3a9#file-wait-go)  hosted with ❤ by  [GitHub](https://github.com/)

These methods are useful to retrieve the destination actors response and proceed own logic but are not usually preferred because the idea of such synchronous execution conflicts with the nature of actor model: concurrent computation.

# Future.PipeTo()

While  `Future.Wait()`  and  `Future.Result()`  block until timeout or task completion,  `Future.PipeTo()`  asynchronously sends the result of computation to another actor. This can be a powerful tool when only the origination actor knows which actor should receive the result of a worker actor’s task; Actor A delegates a task to worker actor B but B does not know to what actor to pass the result message to. One important thing is that the message is transfered to the destination actor when and only when the comptation completes before it times out. Otherwise the response is sent to the dead letter mailbox.  
Becausee this works in an asynchronous manner, origination actor can handle incoming messages right after dispatching tasks to worker actors no matter how long the worker actors take to respond.

![](https://1.bp.blogspot.com/-4N-KIngq5PI/W_fgL4GrNpI/AAAAAAAAA4c/zVcppmuvP4QIRzLc0Dw7jLGPo0rM-5mZACPcBGAYYCw/s1600/timeline2.png)

package main

import (

"github.com/AsynkronIT/protoactor-go/actor"

"github.com/AsynkronIT/protoactor-go/router"

"log"

"os"

"os/signal"

"syscall"

"time"

)

type pong struct {

count uint

}

type ping struct {

count uint

}

type pingActor struct {

count uint

pongPid *actor.PID

}

func (p *pingActor) Receive(ctx actor.Context) {

switch msg := ctx.Message().(type) {

case struct{}:

p.count++

// Output becomes somewhat like below.

// See a diagram at https://raw.githubusercontent.com/oklahomer/protoactor-go-future-example/master/docs/pipe/timeline.png

// 2018/10/14 14:20:36 Received pong message &main.pong{count:1}

// 2018/10/14 14:20:39 Received pong message &main.pong{count:4}

// 2018/10/14 14:20:39 Received pong message &main.pong{count:3}

// 2018/10/14 05:20:39 [ACTOR] [DeadLetter] pid="nonhost/future$e" message=&{'\x02'} sender="nil"

// 2018/10/14 14:20:42 Received pong message &main.pong{count:7}

// 2018/10/14 14:20:42 Received pong message &main.pong{count:6}

// 2018/10/14 05:20:42 [ACTOR] [DeadLetter] pid="nonhost/future$h" message=&{'\x05'} sender="nil"

// 2018/10/14 14:20:45 Received pong message &main.pong{count:10}

// 2018/10/14 14:20:45 Received pong message &main.pong{count:9}

// 2018/10/14 05:20:45 [ACTOR] [DeadLetter] pid="nonhost/future$k" message=&{'\b'} sender="nil"

message := &ping{

count: p.count,

}

p.

pongPid.

RequestFuture(message, 2500*time.Millisecond).

PipeTo(ctx.Self())

case *pong:

log.Printf("Received pong message %#v", msg)

}

}

func main() {

pongProps := router.NewRoundRobinPool(10).

WithFunc(func(ctx actor.Context) {

switch msg := ctx.Message().(type) {

case *ping:

var sleep time.Duration

remainder := msg.count % 3

if remainder == 0 {

sleep = 1700 * time.Millisecond

} else if remainder == 1 {

sleep = 300 * time.Millisecond

} else {

sleep = 2900 * time.Millisecond

}

time.Sleep(sleep)

message := &pong{

count: msg.count,

}

ctx.Sender().Tell(message)

}

})

pongPid := actor.Spawn(pongProps)

pingProps := actor.FromProducer(func() actor.Actor {

return &pingActor{

count: 0,

pongPid: pongPid,

}

})

pingPid := actor.Spawn(pingProps)

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

return

log.Print("Finish")

}

}

}

[view raw](https://gist.github.com/oklahomer/a8cf524b8b5e71939f8ba93ae951718e/raw/7c98e932579ff880f30ca2c95b249540b94262cc/pipeto.go)[pipeto.go](https://gist.github.com/oklahomer/a8cf524b8b5e71939f8ba93ae951718e#file-pipeto-go)  hosted with ❤ by  [GitHub](https://github.com/)

# Context.AwaitFuture()

The task execution is done in an asynchronous manner like  `Future.PipeTo`  but, because this can refer to contextual information, a callback function is still called even when the computation times out.  `Context.Message()`  contains the same message as when the origination actor’s  `Actor.Receive()`  is called so a developer does not have to add a workaround to copy the message to refer from the callback function.

![](https://1.bp.blogspot.com/-0DHuQHUzwQE/W_fgL95dpPI/AAAAAAAAA4g/mF48pNlyfNUhAL97EJjwHIqf51luq0n5QCPcBGAYYCw/s1600/timeline3.png)

package main

import (

"github.com/AsynkronIT/protoactor-go/actor"

"github.com/AsynkronIT/protoactor-go/router"

"log"

"os"

"os/signal"

"syscall"

"time"

)

type tick struct {

count int

}

type pong struct {

count int

}

type ping struct {

count int

}

type pingActor struct {

pongPid *actor.PID

}

func (p *pingActor) Receive(ctx actor.Context) {

switch msg := ctx.Message().(type) {

case *tick:

// Output becomes somewhat like below.

// See a diagram at https://raw.githubusercontent.com/oklahomer/protoactor-go-future-example/master/docs/await_future/timeline.png

//

// 2018/10/14 16:10:49 Received pong response: &main.pong{count:1}

// 2018/10/14 16:10:52 Received pong response: &main.pong{count:4}

// 2018/10/14 16:10:52 Failed to handle: 2. message: &main.tick{count:2}.

// 2018/10/14 16:10:53 Received pong response: &main.pong{count:3}

// 2018/10/14 07:10:53 [ACTOR] [DeadLetter] pid="nonhost/future$e" message=&{'\x02'} sender="nil"

// 2018/10/14 16:10:55 Received pong response: &main.pong{count:7}

// 2018/10/14 16:10:55 Failed to handle: 5. message: &main.tick{count:5}.

// 2018/10/14 16:10:56 Received pong response: &main.pong{count:6}

// 2018/10/14 07:10:56 [ACTOR] [DeadLetter] pid="nonhost/future$h" message=&{'\x05'} sender="nil"

// 2018/10/14 16:10:58 Received pong response: &main.pong{count:10}

// 2018/10/14 16:10:58 Failed to handle: 8. message: &main.tick{count:8}.

// 2018/10/14 16:10:59 Received pong response: &main.pong{count:9}

// 2018/10/14 07:10:59 [ACTOR] [DeadLetter] pid="nonhost/future$k" message=&{'\b'} sender="nil"

message := &ping{

count: msg.count,

}

future := p.pongPid.RequestFuture(message, 2500*time.Millisecond)

cnt := msg.count

ctx.AwaitFuture(future, func(res interface{}, err error) {

if err != nil {

// Context.Message() returns the exact message that was present on Context.AwaitFuture call.

// ref. https://github.com/AsynkronIT/protoactor-go/blob/3992780c0af683deb5ec3746f4ec5845139c6e42/actor/local_context.go#L289

log.Printf("Failed to handle: %d. message: %#v.", cnt, ctx.Message())

return

}

switch res.(type) {

case *pong:

log.Printf("Received pong response: %#v", res)

default:

log.Printf("Received unexpected response: %#v", res)

}

})

}

}

func main() {

pongProps := router.NewRoundRobinPool(10).

WithFunc(func(ctx actor.Context) {

switch msg := ctx.Message().(type) {

case *ping:

var sleep time.Duration

remainder := msg.count % 3

if remainder == 0 {

sleep = 1700 * time.Millisecond

} else if remainder == 1 {

sleep = 300 * time.Millisecond

} else {

sleep = 2900 * time.Millisecond

}

time.Sleep(sleep)

message := &pong{

count: msg.count,

}

ctx.Sender().Tell(message)

}

})

pongPid := actor.Spawn(pongProps)

pingProps := actor.FromProducer(func() actor.Actor {

return &pingActor{

pongPid: pongPid,

}

})

pingPid := actor.Spawn(pingProps)

finish := make(chan os.Signal, 1)

signal.Notify(finish, os.Interrupt)

signal.Notify(finish, syscall.SIGTERM)

ticker := time.NewTicker(1 * time.Second)

defer ticker.Stop()

count := 0

for {

select {

case <-ticker.C:

count++

pingPid.Tell(&tick{count: count})

case <-finish:

return

log.Print("Finish")

}

}

}

[view raw](https://gist.github.com/oklahomer/cef28ea5911431a4a47fa69f8b3f35ed/raw/2f7d4f99ec8304b482aff79f0a7986004bca43ef/await_future.go)[await_future.go](https://gist.github.com/oklahomer/cef28ea5911431a4a47fa69f8b3f35ed#file-await_future-go)  hosted with ❤ by  [GitHub](https://github.com/)

# Conclusion

As described in above sections, Future provides various methods to synchronize concurrent execution. While concurrent execution is the core of actor model, these come in handy to synchronize concurrent execution with minimal cost.


### [Golang] protoactor-go 101：actor.Future如何同步并发任务执行

细粒度的演员专注于自己的任务，并与他人沟通，以完成一个更大的任务。  这就是精心设计的演员系统的运作方式。  因为每个actor处理传入任务的较小部分，将其传递给另一个，然后继续处理下一个传入任务，actor可以以并发方式有效地工作，以在相同的时间内完成更多任务。  要将一个actor的作业执行结果传递给另一个actor，或者在另一个actor的执行完成时执行任务，actor.Future机制就派上用场了。

-   [介绍未来](https://translate.googleusercontent.com/translate_c?depth=1&hl=zh-CN&prev=search&rurl=translate.google.com&sl=en&sp=nmt4&u=https://blog.oklahome.net/2018/11/protoactor-go-how-future-works.html&xid=17259,15700023,15700124,15700186,15700191,15700201,15700237,15700242,15700248&usg=ALkJrhjsT-EjsssLRFpheUzv-cZruQzsHA#toc_0)
    -   [如何在引擎盖下工作](https://translate.googleusercontent.com/translate_c?depth=1&hl=zh-CN&prev=search&rurl=translate.google.com&sl=en&sp=nmt4&u=https://blog.oklahome.net/2018/11/protoactor-go-how-future-works.html&xid=17259,15700023,15700124,15700186,15700191,15700201,15700237,15700242,15700248&usg=ALkJrhjsT-EjsssLRFpheUzv-cZruQzsHA#toc_1)
-   [Future.Wait（）/ Future.Result（）](https://translate.googleusercontent.com/translate_c?depth=1&hl=zh-CN&prev=search&rurl=translate.google.com&sl=en&sp=nmt4&u=https://blog.oklahome.net/2018/11/protoactor-go-how-future-works.html&xid=17259,15700023,15700124,15700186,15700191,15700201,15700237,15700242,15700248&usg=ALkJrhjsT-EjsssLRFpheUzv-cZruQzsHA#toc_2)
-   [Future.PipeTo（）](https://translate.googleusercontent.com/translate_c?depth=1&hl=zh-CN&prev=search&rurl=translate.google.com&sl=en&sp=nmt4&u=https://blog.oklahome.net/2018/11/protoactor-go-how-future-works.html&xid=17259,15700023,15700124,15700186,15700191,15700201,15700237,15700242,15700248&usg=ALkJrhjsT-EjsssLRFpheUzv-cZruQzsHA#toc_3)
-   [Context.AwaitFuture（）](https://translate.googleusercontent.com/translate_c?depth=1&hl=zh-CN&prev=search&rurl=translate.google.com&sl=en&sp=nmt4&u=https://blog.oklahome.net/2018/11/protoactor-go-how-future-works.html&xid=17259,15700023,15700124,15700186,15700191,15700201,15700237,15700242,15700248&usg=ALkJrhjsT-EjsssLRFpheUzv-cZruQzsHA#toc_4)
-   [结论](https://translate.googleusercontent.com/translate_c?depth=1&hl=zh-CN&prev=search&rurl=translate.google.com&sl=en&sp=nmt4&u=https://blog.oklahome.net/2018/11/protoactor-go-how-future-works.html&xid=17259,15700023,15700124,15700186,15700191,15700201,15700237,15700242,15700248&usg=ALkJrhjsT-EjsssLRFpheUzv-cZruQzsHA#toc_5)

# 介绍未来

在这种情况下，“未来”的基本思想非常类似于`future`和许多现代编程语言为协调并发执行而实现的`promise`  。

例如，  [`Future`](https://translate.googleusercontent.com/translate_c?depth=1&hl=zh-CN&prev=search&rurl=translate.google.com&sl=en&sp=nmt4&u=https://docs.oracle.com/javase/8/docs/api/index.html%3Fjava/util/concurrent/Future.html&xid=17259,15700023,15700124,15700186,15700191,15700201,15700237,15700242,15700248&usg=ALkJrhgRmIgILuPzV9ehdnm-sUqMLEec7Q)的Javadoc如下所示：

> `Future`表示异步计算的结果。  提供方法以检查计算是否完成，等待其完成，以及检索计算结果。

在protoactor-go中，  `actor.Future`提供了以阻塞方式等待目标actor的响应的方法，以非阻塞方式将一个actor的响应传递给另一个actor，并在目标actor的响应到达时执行回调函数。

## 如何在引擎盖下工作

protoactor-go的Future机制的实现由[`actor.Future`](https://github.com/AsynkronIT/protoactor-go/blob/f373762276d85e13d8b2999e58e36d01d44e569d/actor/future.go#L35-L45)和[`actor.futureProcess`](https://github.com/AsynkronIT/protoactor-go/blob/f373762276d85e13d8b2999e58e36d01d44e569d/actor/future.go#L109-L112)组成，其中`actor.Future`提供常见的Future方法，而`actor.futureProcess`包含`actor.Future`并作为`actor.Process`  。  开发人员可以调用`Context.RequestFuture()`或`PID.RequestFuture()`而不是常用的`Context.Request()`或`PID.Request()`来获取表示未确定结果的`actor.Future`  。

在这些方法调用中，调用`actor.NewFuture()`  ，  `actor.NewFuture()`首选超时持续时间作为参数。  在`actor.NewFuture()`  ，  `actor.Future`及其包装器 -  `actor.futureProcess`  - 都被构造，  `actor.futureProcess`被注册到protoactor-go的内部进程注册表以供稍后的引用和`actor.Future`返回。

如下面的代码片段所示，  `actor.Future`的`actor.PID`被设置为请求消息的发送者。  因此，当`actor.Future`响应发送者时，响应实际上被发送给了`actor.Future`的`actor.PID`  。当`actor.Future`收到响应时，将设置Future的结果并使订户可以使用。

 `func (ctx *localContext)  RequestFuture(pid *PID, message interface{}, timeout time.Duration)  *Future  { future :=  NewFuture(timeout) env :=  &MessageEnvelope{  Header:  nil,  Message: message,  Sender: future.PID(),  } ctx.sendUserMessage(pid, env)  return future }` 

这些方法返回的`actor.Future`的用法将在后面的章节中详细介绍示例代码。  所有示例代码均可在[github.com/oklahomer/protoactor-go-future-example上获得](https://translate.googleusercontent.com/translate_c?depth=1&hl=zh-CN&prev=search&rurl=translate.google.com&sl=en&sp=nmt4&u=https://github.com/oklahomer/protoactor-go-future-example&xid=17259,15700023,15700124,15700186,15700191,15700201,15700237,15700242,15700248&usg=ALkJrhiarpnhytNk7NDjX4qPWkZxj0BEoQ)  。

# Future.Wait（）/ Future.Result（）

要等到执行超时或目标actor响应，请使用`Future.Wait()`或`Future.Result()`  。  它们都在内部调用阻塞私有方法`Future.wait()`  ，直到预配置的超时持续时间通过或执行完成为止。  唯一的区别是是否返回计算结果;  `Future.Wait()`只是像[`WaitGroup.Wait()`](https://translate.googleusercontent.com/translate_c?depth=1&hl=zh-CN&prev=search&rurl=translate.google.com&sl=en&sp=nmt4&u=https://golang.org/pkg/sync/&xid=17259,15700023,15700124,15700186,15700191,15700201,15700237,15700242,15700248&usg=ALkJrhhrmn33NZ3m6d09R-9J8vqGuxCR5Q#WaitGroup.Wait)一样等待完成，而`Future.Result()`等待完成，另外返回计算结果，顾名思义。

![](https://2.bp.blogspot.com/-1UlqPWNdFaY/W_fgLwxkgnI/AAAAAAAAA4Y/TwcDNHeR9Gg0QPALyRoHPvkVuh4udkysgCPcBGAYYCw/s1600/timeline.png)

package main

import (

"github.com/AsynkronIT/protoactor-go/actor"

"log"

"os"

"os/signal"

"syscall"

"time"

)

type pong struct {

}

type ping struct {

}

type pongActor struct {

timeOut bool

}

func (p *pongActor) Receive(ctx actor.Context) {

// Dead letter occurs because the PID of Future process ends and goes away when Future times out

// so the pongActor fails to send message.

switch ctx.Message().(type) {

case *ping:

var sleep time.Duration

if p.timeOut {

sleep = 2500 * time.Millisecond

p.timeOut = false

} else {

sleep = 300 * time.Millisecond

p.timeOut = true

}

time.Sleep(sleep)

ctx.Sender().Tell(&pong{})

}

}

type pingActor struct {

pongPid *actor.PID

}

func (p *pingActor) Receive(ctx actor.Context) {

switch ctx.Message().(type) {

case struct{}:

// Output becomes somewhat like below.

// See a diagram at https://raw.githubusercontent.com/oklahomer/protoactor-go-future-example/master/docs/wait/timeline.png

//

// 2018/10/13 17:03:22 Received pong message &main.pong{}

// 2018/10/13 17:03:24 Timed out

// 2018/10/13 08:03:26 [ACTOR] [DeadLetter] pid="nonhost/future$4" message=&{} sender="nil"

// 2018/10/13 17:03:26 Received pong message &main.pong{}

// 2018/10/13 17:03:28 Timed out

// 2018/10/13 08:03:30 [ACTOR] [DeadLetter] pid="nonhost/future$6" message=&{} sender="nil"

// 2018/10/13 17:03:30 Received pong message &main.pong{}

future := p.pongPid.RequestFuture(&ping{}, 1*time.Second)

// Future.Result internally waits until response comes or times out

result, err := future.Result()

if err != nil {

log.Print("Timed out")

return

}

log.Printf("Received pong message %#v", result)

}

}

func main() {

pongProps := actor.FromProducer(func() actor.Actor {

return &pongActor{}

})

pongPid := actor.Spawn(pongProps)

pingProps := actor.FromProducer(func() actor.Actor {

return &pingActor{

pongPid: pongPid,

}

})

pingPid := actor.Spawn(pingProps)

finish := make(chan os.Signal, 1)

signal.Notify(finish, os.Interrupt)

signal.Notify(finish, syscall.SIGTERM)

ticker := time.NewTicker(2 * time.Second)

defer ticker.Stop()

for {

select {

case <-ticker.C:

pingPid.Tell(struct{}{})

case <-finish:

return

log.Print("Finish")

}

}

}

[view raw](https://gist.github.com/oklahomer/96001c239d871890087e673d5908c3a9/raw/236c400f2004955211ccbc9dbf27b65c6096a264/wait.go)[wait.go](https://gist.github.com/oklahomer/96001c239d871890087e673d5908c3a9#file-wait-go)  hosted with ❤ by  [GitHub](https://github.com/)

这些方法对于检索目标actor响应和继续自己的逻辑很有用，但通常不是优选的，因为这种同步执行的想法与actor模型的性质冲突：并发计算。

# Future.PipeTo（）

虽然`Future.Wait()`和`Future.Result()`阻塞直到超时或任务完成，但`Future.PipeTo()`异步地将计算结果发送给另一个actor。  当只有创始人知道哪个演员应该接收工人演员的任务结果时，这可以是一个强大的工具;  Actor A将任务委托给worker actor B，但B不知道将结果消息传递给哪个actor。  一个重要的事情是，只有当算法在超时之前完成时，消息才会转移到目标actor。否则，响应将发送到死信邮箱。  
因为它以异步方式工作，所以无论工作者演员响应多长时间，始发者都可以在将任务分派给工作者之后立即处理传入消息。

![](https://1.bp.blogspot.com/-4N-KIngq5PI/W_fgL4GrNpI/AAAAAAAAA4c/zVcppmuvP4QIRzLc0Dw7jLGPo0rM-5mZACPcBGAYYCw/s1600/timeline2.png)

package main

import (

"github.com/AsynkronIT/protoactor-go/actor"

"github.com/AsynkronIT/protoactor-go/router"

"log"

"os"

"os/signal"

"syscall"

"time"

)

type pong struct {

count uint

}

type ping struct {

count uint

}

type pingActor struct {

count uint

pongPid *actor.PID

}

func (p *pingActor) Receive(ctx actor.Context) {

switch msg := ctx.Message().(type) {

case struct{}:

p.count++

// Output becomes somewhat like below.

// See a diagram at https://raw.githubusercontent.com/oklahomer/protoactor-go-future-example/master/docs/pipe/timeline.png

// 2018/10/14 14:20:36 Received pong message &main.pong{count:1}

// 2018/10/14 14:20:39 Received pong message &main.pong{count:4}

// 2018/10/14 14:20:39 Received pong message &main.pong{count:3}

// 2018/10/14 05:20:39 [ACTOR] [DeadLetter] pid="nonhost/future$e" message=&{'\x02'} sender="nil"

// 2018/10/14 14:20:42 Received pong message &main.pong{count:7}

// 2018/10/14 14:20:42 Received pong message &main.pong{count:6}

// 2018/10/14 05:20:42 [ACTOR] [DeadLetter] pid="nonhost/future$h" message=&{'\x05'} sender="nil"

// 2018/10/14 14:20:45 Received pong message &main.pong{count:10}

// 2018/10/14 14:20:45 Received pong message &main.pong{count:9}

// 2018/10/14 05:20:45 [ACTOR] [DeadLetter] pid="nonhost/future$k" message=&{'\b'} sender="nil"

message := &ping{

count: p.count,

}

p.

pongPid.

RequestFuture(message, 2500*time.Millisecond).

PipeTo(ctx.Self())

case *pong:

log.Printf("Received pong message %#v", msg)

}

}

func main() {

pongProps := router.NewRoundRobinPool(10).

WithFunc(func(ctx actor.Context) {

switch msg := ctx.Message().(type) {

case *ping:

var sleep time.Duration

remainder := msg.count % 3

if remainder == 0 {

sleep = 1700 * time.Millisecond

} else if remainder == 1 {

sleep = 300 * time.Millisecond

} else {

sleep = 2900 * time.Millisecond

}

time.Sleep(sleep)

message := &pong{

count: msg.count,

}

ctx.Sender().Tell(message)

}

})

pongPid := actor.Spawn(pongProps)

pingProps := actor.FromProducer(func() actor.Actor {

return &pingActor{

count: 0,

pongPid: pongPid,

}

})

pingPid := actor.Spawn(pingProps)

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

return

log.Print("Finish")

}

}

}

[view raw](https://gist.github.com/oklahomer/a8cf524b8b5e71939f8ba93ae951718e/raw/7c98e932579ff880f30ca2c95b249540b94262cc/pipeto.go)[pipeto.go](https://gist.github.com/oklahomer/a8cf524b8b5e71939f8ba93ae951718e#file-pipeto-go)  hosted with ❤ by  [GitHub](https://github.com/)

# Context.AwaitFuture（）

任务执行以`Future.PipeTo`等异步方式完成，但由于这可以引用上下文信息，因此即使计算超时，仍会调用回调函数。  `Context.Message()`包含与调用原始actor的`Actor.Receive()`时相同的消息，因此开发人员不必添加变通方法来复制要从回调函数引用的消息。

![](https://1.bp.blogspot.com/-0DHuQHUzwQE/W_fgL95dpPI/AAAAAAAAA4g/mF48pNlyfNUhAL97EJjwHIqf51luq0n5QCPcBGAYYCw/s1600/timeline3.png)

package main

import (

"github.com/AsynkronIT/protoactor-go/actor"

"github.com/AsynkronIT/protoactor-go/router"

"log"

"os"

"os/signal"

"syscall"

"time"

)

type tick struct {

count int

}

type pong struct {

count int

}

type ping struct {

count int

}

type pingActor struct {

pongPid *actor.PID

}

func (p *pingActor) Receive(ctx actor.Context) {

switch msg := ctx.Message().(type) {

case *tick:

// Output becomes somewhat like below.

// See a diagram at https://raw.githubusercontent.com/oklahomer/protoactor-go-future-example/master/docs/await_future/timeline.png

//

// 2018/10/14 16:10:49 Received pong response: &main.pong{count:1}

// 2018/10/14 16:10:52 Received pong response: &main.pong{count:4}

// 2018/10/14 16:10:52 Failed to handle: 2. message: &main.tick{count:2}.

// 2018/10/14 16:10:53 Received pong response: &main.pong{count:3}

// 2018/10/14 07:10:53 [ACTOR] [DeadLetter] pid="nonhost/future$e" message=&{'\x02'} sender="nil"

// 2018/10/14 16:10:55 Received pong response: &main.pong{count:7}

// 2018/10/14 16:10:55 Failed to handle: 5. message: &main.tick{count:5}.

// 2018/10/14 16:10:56 Received pong response: &main.pong{count:6}

// 2018/10/14 07:10:56 [ACTOR] [DeadLetter] pid="nonhost/future$h" message=&{'\x05'} sender="nil"

// 2018/10/14 16:10:58 Received pong response: &main.pong{count:10}

// 2018/10/14 16:10:58 Failed to handle: 8. message: &main.tick{count:8}.

// 2018/10/14 16:10:59 Received pong response: &main.pong{count:9}

// 2018/10/14 07:10:59 [ACTOR] [DeadLetter] pid="nonhost/future$k" message=&{'\b'} sender="nil"

message := &ping{

count: msg.count,

}

future := p.pongPid.RequestFuture(message, 2500*time.Millisecond)

cnt := msg.count

ctx.AwaitFuture(future, func(res interface{}, err error) {

if err != nil {

// Context.Message() returns the exact message that was present on Context.AwaitFuture call.

// ref. https://github.com/AsynkronIT/protoactor-go/blob/3992780c0af683deb5ec3746f4ec5845139c6e42/actor/local_context.go#L289

log.Printf("Failed to handle: %d. message: %#v.", cnt, ctx.Message())

return

}

switch res.(type) {

case *pong:

log.Printf("Received pong response: %#v", res)

default:

log.Printf("Received unexpected response: %#v", res)

}

})

}

}

func main() {

pongProps := router.NewRoundRobinPool(10).

WithFunc(func(ctx actor.Context) {

switch msg := ctx.Message().(type) {

case *ping:

var sleep time.Duration

remainder := msg.count % 3

if remainder == 0 {

sleep = 1700 * time.Millisecond

} else if remainder == 1 {

sleep = 300 * time.Millisecond

} else {

sleep = 2900 * time.Millisecond

}

time.Sleep(sleep)

message := &pong{

count: msg.count,

}

ctx.Sender().Tell(message)

}

})

pongPid := actor.Spawn(pongProps)

pingProps := actor.FromProducer(func() actor.Actor {

return &pingActor{

pongPid: pongPid,

}

})

pingPid := actor.Spawn(pingProps)

finish := make(chan os.Signal, 1)

signal.Notify(finish, os.Interrupt)

signal.Notify(finish, syscall.SIGTERM)

ticker := time.NewTicker(1 * time.Second)

defer ticker.Stop()

count := 0

for {

select {

case <-ticker.C:

count++

pingPid.Tell(&tick{count: count})

case <-finish:

return

log.Print("Finish")

}

}

}

[view raw](https://gist.github.com/oklahomer/cef28ea5911431a4a47fa69f8b3f35ed/raw/2f7d4f99ec8304b482aff79f0a7986004bca43ef/await_future.go)[await_future.go](https://gist.github.com/oklahomer/cef28ea5911431a4a47fa69f8b3f35ed#file-await_future-go)  hosted with ❤ by  [GitHub](https://github.com/)

# 结论

如上一节所述，Future提供了同步并发执行的各种方法。  虽然并发执行是actor模型的核心，但它们可以方便地以最小的成本同步并发执行。
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTEyOTQ0MTk5NTgsMTA2MzQ3NzA1MCwxNT
kzNDE5Mjc1LC0xNjM2Njg4MDU3LC0zNDAyOTE4OCwyMTM3MTAz
ODc4XX0=
-->