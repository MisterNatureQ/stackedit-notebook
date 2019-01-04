# [[Golang] protoactor-go 101: How actor.Future works to synchronize concurrent task execution](https://blog.oklahome.net/2018/11/protoactor-go-how-future-works.html)

Fine-grained actors concentrate on their own tasks and communicate with others to accomplish a bigger task as a whole. That is how a well-designed actor system works. Because each actor handles a smaller part of an incoming task, pass it to another and then proceed to work on the next incoming task, actors can efficiently work in a concurrent manner to accomplish more tasks in the same amount of time. To pass a result of one actor’s job execution to another or to execute a task when another actor’s execution is done, actor.Future mechanism comes in handy.

-   [Introducing Future](https://blog.oklahome.net/2018/11/protoactor-go-how-future-works.html#toc_0)
    -   [How this works under the hood](https://blog.oklahome.net/2018/11/protoactor-go-how-future-works.html#toc_1)
-   [Future.Wait() / Future.Result()](https://blog.oklahome.net/2018/11/protoactor-go-how-future-works.html#toc_2)
-   [Future.PipeTo()](https://blog.oklahome.net/2018/11/protoactor-go-how-future-works.html#toc_3)
-   [Context.AwaitFuture()](https://blog.oklahome.net/2018/11/protoactor-go-how-future-works.html#toc_4)
-   [Conclusion](https://blog.oklahome.net/2018/11/protoactor-go-how-future-works.html#toc_5)

# Introducing Future

The basic idea of “future” in this context is quite similar to that of  `future`  and  `promise`  implemented by many modern programming languages to coordinate concurrent executions.

For example,  [`Future`](https://docs.oracle.com/javase/8/docs/api/index.html?java/util/concurrent/Future.html)’s Javadoc reads as below:

> A`Future`represents the result of an asynchronous computation. Methods are provided to check if the computation is complete, to wait for its completion, and to retrieve the result of the computation.

In protoactor-go,  `actor.Future`  provides methods to wait for destination actor’s response in a blocking manner, to pipe one actor’s response to another in a non-blocking manner and to execute a callback function when destination actor’s response arrives.

## How this works under the hood

The implementation of protoactor-go’s Future mechanism is composed of  [`actor.Future`](https://github.com/AsynkronIT/protoactor-go/blob/f373762276d85e13d8b2999e58e36d01d44e569d/actor/future.go#L35-L45)  and  [`actor.futureProcess`](https://github.com/AsynkronIT/protoactor-go/blob/f373762276d85e13d8b2999e58e36d01d44e569d/actor/future.go#L109-L112), where  `actor.Future`  provides common Future methods while  `actor.futureProcess`  wraps  `actor.Future`  and works as a  `actor.Process`. A developer may call  `Context.RequestFuture()`  or  `PID.RequestFuture()`  instead of commonly used  `Context.Request()`or  `PID.Request()`  to acquire  `actor.Future`  that represents a non-determined result.

In those method calls,  `actor.NewFuture()`  is called with preferred timeout duration as an argument. In  `actor.NewFuture()`,  `actor.Future`  and its wrapper –  `actor.futureProcess`  – are both constructed,  `actor.futureProcess`  is registered to protoactor-go’s internal process registry for a later reference and  `actor.Future`  is returned.

As depicted in the below code fragment,  `actor.Future`’s  `actor.PID`  is set as a  `Sender`  of the requesting message. So when the receiving actor responds to the sender, the response is actually sent to the  `actor.Future`’s  `actor.PID`. When  `actor.Future`  receives the response, the result of the Future is set and becomes available to the subscriber.

```go
func (ctx *localContext) RequestFuture(pid *PID, message interface{}, timeout time.Duration) *Future {  
   future := NewFuture(timeout)  
   env := &MessageEnvelope{  
      Header:  nil,  
  Message: message,  
  Sender:  future.PID(),  
  }  
   ctx.sendUserMessage(pid, env)  
  
   return future  
}
```

The usage of the  `actor.Future`  returned by those methods are covered in later sections with detailed example codes. All example codes are available at  [github.com/oklahomer/protoactor-go-future-example](https://github.com/oklahomer/protoactor-go-future-example).

# Future.Wait() / Future.Result()

To wait until the execution times out or the destination actor responds, use  `Future.Wait()`  or  `Future.Result()`. They both internally call a blocking private method,  `Future.wait()`, to block till the preconfigured timeout duration passes or the execution completes. The only difference is whether to return the result of the computation;  `Future.Wait()`  simply waits for completion just like  [`WaitGroup.Wait()`](https://golang.org/pkg/sync/#WaitGroup.Wait)  while  `Future.Result()`  waits for completion and additionally returns the result ot the computation as its name suggests.

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
<!--stackedit_data:
eyJoaXN0b3J5IjpbMjEzNzEwMzg3OF19
-->