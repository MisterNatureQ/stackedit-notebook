# [[Golang] protoactor-go 101: How actors communicate with each other](https://blog.oklahome.net/2018/09/protoactor-go-messaging-protocol.html)

Designing actor-based program is all about dividing tasks into smaller pieces. Fine-grained actors concentrate on their tasks, collaborate with other actors and accomplish a big task as a whole. Hence mastering actors’ communication mechanism and modeling well-defined messages are always the keys to designing an actor system. This article describes protoactor-go’s actor categories, their messaging methods and how those methods differ on referencing sender actors.

设计 actor-based 的程序就是将任务分成更小的部分。  细粒度的 actors  专注于他们的任务，与其他 actors 合作，完成一项重大任务。  因此，掌握 actors 的沟通机制和对定义明确的消息进行建模始终是设计 actors  系统的关键。  本文描述了protoactor-go的actor类别，它们的消息传递方法以及这些方法在引用发送方actor方面的不同之处。

See my previous article,  [[Golang] protoactor-go 101: Introduction to golang’s actor model implementation](https://blog.oklahome.net/2018/07/protoactor-go-introduction.html), for protoactor-go’s basic concepts and terms.

-   [TL;DR](https://blog.oklahome.net/2018/09/protoactor-go-messaging-protocol.html#toc_0)
-   [Example codes](https://blog.oklahome.net/2018/09/protoactor-go-messaging-protocol.html#toc_1)
-   [Premise: Three major kinds of actors](https://blog.oklahome.net/2018/09/protoactor-go-messaging-protocol.html#toc_2)
-   [Communication methods](https://blog.oklahome.net/2018/09/protoactor-go-messaging-protocol.html#toc_3)
    -   [Tell() tells nothing about the sender](https://blog.oklahome.net/2018/09/protoactor-go-messaging-protocol.html#toc_4)
    -   [Request() lets a recipient request for the sender reference](https://blog.oklahome.net/2018/09/protoactor-go-messaging-protocol.html#toc_5)
    -   [RequestFuture() only has its future](https://blog.oklahome.net/2018/09/protoactor-go-messaging-protocol.html#toc_6)
    -   [Cluster grain’s unique RPC based messaging](https://blog.oklahome.net/2018/09/protoactor-go-messaging-protocol.html#toc_7)
-   [Conclusion](https://blog.oklahome.net/2018/09/protoactor-go-messaging-protocol.html#toc_8)

# TL;DR     Too long; Didn't read（太长，所以没有看）

While there are several kinds of actors, those actors share a unified interface to communicate with each other. Various methods are provided for their communication, but always use Request() to acknowledge the recipient actor who the sender actor is. When that is not an option, include the sender actor’s [actor.PID](https://github.com/AsynkronIT/protoactor-go/blob/3992780c0af683deb5ec3746f4ec5845139c6e42/actor/pid.go#L12-L17) in the sending message.

虽然有几种 actors ，但这些 actors 共享一个统一的接口来相互沟通。  为它们通信提供了各种方法，但总是使用Request（）来告知接收方actor 发送方的actor  。  如果这不是一个选项，  请在发送消息中包含发送方actor的[actor.PID](https://github.com/AsynkronIT/protoactor-go/blob/3992780c0af683deb5ec3746f4ec5845139c6e42/actor/pid.go#L12-L17)  。

# Example codes

Example codes that cover all communication means for all actor implementations are located at  [github.com/oklahomer/protoactor-go-sender-example](https://github.com/oklahomer/protoactor-go-sender-example). Minimal examples are introduced in this article for description, but visit this repository for comprehensive examples.

覆盖所有actor实现的所有通信手段的示例代码位于[github.com/oklahomer/protoactor-go-sender-example](https://translate.googleusercontent.com/translate_c?depth=1&hl=zh-CN&rurl=translate.google.com&sl=en&sp=nmt4&tl=zh-CN&u=https://github.com/oklahomer/protoactor-go-sender-example&xid=17259,15700023,15700124,15700186,15700191,15700201,15700237,15700242,15700248&usg=ALkJrhinCWgnXZUiXeTubLyMHvTqQr8MbA)  。  本文中介绍了最小的示例以供说明，但请访问此存储库以获取全面的示例。

# Premise: Three major kinds of actors 引出：三大类actors 

protoactor-go comes with three kinds of actors: local, remote and cluster grain.
protoactor-go有三种 actors：本地，远程和集群粒子。
[![](https://3.bp.blogspot.com/-Rn0COjNd0qQ/W6hy96hicDI/AAAAAAAAA2U/HjaHhdsEOLAYt3BrQT8GXFkKnjuxUwNlgCEwYBhgL/s1600/components.png)](https://3.bp.blogspot.com/-Rn0COjNd0qQ/W6hy96hicDI/AAAAAAAAA2U/HjaHhdsEOLAYt3BrQT8GXFkKnjuxUwNlgCEwYBhgL/s1600/components.png)

-   Local … Those actors located in the same process.  
    本地......那些actors  位于同一个进程中 并不是actor process
-   Remote … Actors located in different processes or servers. An actor is considered to be “local” when addressed from within the same process; while this is “remote” when addressed across a network. Because a message is sent over a network, message serialization is required. Protocol Buffers is used for this task in protoactor-go.  
    远程...位于不同进程或服务器中的Actor。  当收信地址 在同一过程中处理时，Actor被认为是“local”;  虽然这是通过网络寻址的“远程”。  由于消息是通过网络发送的，因此需要进行消息序列化。  Protocol Buffers用于protoactor-go中的此任务。
    
-   Cluster grain … A kind of remote actor but the lifecycle and other complexity are taken care of by protoactor-go library. Cluster topology is managed by  [consul](https://www.consul.io/)  and a grain can be addressed over a network. Consul manages the cluster membership and the availability of each node.  
    集群粒子......一种remote actor，但生命周期和其他复杂性由protoactor-go库来处理。  群集拓扑由[consul](https://www.consul.io/)管理，粒子可以通过网络进行寻址。  Consul管理集群成员资格和每个节点的可用性。

Thanks to the location transparency, an actor can communicate with other actors in the same way without worrying about where the recipient actors are located at. In addition to those basic communication means, a cluster grain has an extra mechanism to provide RPC based interface.

由于位置透明，演员可以以相同的方式与其他演员沟通，而不必担心收件人演员所在的位置。除了那些基本的通信手段之外，群集粒度还有一个额外的机制来提供基于RPC的接口。

Each actor is encapsulated in an actor.PID instance so developers communicate with actors via methods provided by this actor.PID. (actor.Context also provides equivalent methods, but these can be considered as wrappers for actor.PID’s corresponding methods.) One important thing to remember is that above actors are not the only entities encapsulated in actor.PIDs. As a matter of fact, any actor.Process implementation including mailbox, Future mechanism and others are also encapsulated in actor.PIDs. This may be familiar to those with Erlang background. Understanding this becomes vital when one tries referring to message sender actor. The rest of this article is going to describe each messaging method and how a recipient actor can refer to the sending actor.

每个actor都封装在一个actor.PID实例中，因此开发人员可以通过此actor.PID提供的方法与actor进行通信。  （actor.Context也提供了等效的方法，但是这些方法可以被认为是actor.PID对应方法的包装。）要记住的一件重要事情是上面的actor不是封装在actor.PID中的唯一实体。  事实上，任何actor.Process实现，包括邮箱，Future机制等也都封装在actor.PID中。  这对于具有Erlang背景的人来说可能是熟悉的。  当人们尝试引用消息发送者actor时，理解这一点变得至关重要  本文的其余部分将描述每个消息传递方法以及收件人actor如何引用发送actor。

# Communication methods

Below are the common communication methods – Tell(), Request() and RequestFuture() – and RPC based method for cluster grain. Examples in this article all demonstrate local actor messaging because local and remote actors share a common messaging interface. Visit my  [example repository](https://github.com/oklahomer/protoactor-go-sender-example)  to cover all messaging implementations of local, remote and cluster grain.

## Tell() tells nothing about the sender

To send a message to an actor, one may call actor.PID’s Tell() method. When a message is sent from outside of an actor system by calling PID.Tell(), the recipient actor fails to refer to the sending actor with [Context.Sender()](https://github.com/AsynkronIT/protoactor-go/blob/3992780c0af683deb5ec3746f4ec5845139c6e42/actor/context.go#L16-L17). This is pretty obvious. Because the message is sent from outside, there is no such thing as sending actor. Below is an example:

package main

import (

"github.com/AsynkronIT/protoactor-go/actor"

"time"

)

type ping struct{}

type pong struct{}

func main() {

props := actor.FromFunc(func(ctx actor.Context) {

switch ctx.Message().(type) {

case *ping:

// This fails to get sender

// because the message came

// from outside of actor system

//

// Below execution leads to dead letter

// 2018/09/14 22:40:02 [ACTOR] [DeadLetter] pid="nil" message=&{} sender="nil"

ctx.Respond(&pong{})

// Below execution causes a panic since Sender() returns nil.

// Actor crashes and that causes supervisor to restart this failing actor.

// 2018/09/14 22:40:02 [MAILBOX] [ACTOR] Recovering actor="nonhost/$1" reason="runtime error: invalid memory address or nil pointer dereference" stacktrace="github.com/AsynkronIT/protoactor-go/actor.(*PID).ref:26"

// 2018/09/14 22:40:02 [ACTOR] [SUPERVISION] actor="nonhost/$1" directive="RestartDirective" reason="runtime error: invalid memory address or nil pointer dereference"

ctx.Sender().Tell(&pong{})

}

})

pid := actor.Spawn(props)

pid.Tell(&ping{})

time.Sleep(1 * time.Second) // Just to make sure system ends after actor execution

}

[view raw](https://gist.github.com/oklahomer/2cd42e0c19abf3d10b671f5a98e71571/raw/dafa0c3635da253a3497b614fd215f8a6ee6a052/no-sender.go)[no-sender.go](https://gist.github.com/oklahomer/2cd42e0c19abf3d10b671f5a98e71571#file-no-sender-go)  hosted with ❤ by  [GitHub](https://github.com/)

In the above example, a message is directly sent to an actor from outside of an actor system. Therefore the recipient actor fails to refer to the sending actor. With Akka, this behavior is similar to set [ActorRef#noSender](https://doc.akka.io/japi/akka/2.4.2/akka/actor/ActorRef.html#noSender--) as the second argument of [ActorRef#tell](https://doc.akka.io/japi/akka/2.4.2/akka/actor/ActorRef.html#tell-java.lang.Object-akka.actor.ActorRef-) – when the recipient tries to respond, the message goes to the dead letter mailbox.

When a message is sent from one actor to another, there indeed is a sender-recipient relationship. Recipient actor’s contextual information, actor.Context, appears to provide such information for us. Below is an example code that tries to refer to the sender actor with actor.Context:

package main

import (

"github.com/AsynkronIT/protoactor-go/actor"

"log"

"time"

)

type pong struct {

}

type ping struct {

}

type pingActor struct {

pongPid *actor.PID

}

func (p *pingActor) Receive(ctx actor.Context) {

switch ctx.Message().(type) {

case struct{}:

// Below does not set ctx.Self() as sender,

// and hence the recipient has no knowledge of the sender

// even though the message is sent from another actor via actor.Context.

//

ctx.Tell(p.pongPid, &ping{})

case *pong:

log.Print("Received pong message")

}

}

func main() {

pongProps := actor.FromFunc(func(ctx actor.Context) {

switch ctx.Message().(type) {

case *ping:

log.Print("Received ping message")

// 2018/09/15 02:01:27 [ACTOR] [DeadLetter] pid="nil" message=&{} sender="nil"

ctx.Respond(&pong{})

// 2018/09/15 02:01:27 [MAILBOX] [ACTOR] Recovering actor="nonhost/$1" reason="runtime error: invalid memory address or nil pointer dereference" stacktrace="github.com/AsynkronIT/protoactor-go/actor.(*PID).ref:26"

// 2018/09/15 02:01:27 [ACTOR] [SUPERVISION] actor="nonhost/$1" directive="RestartDirective" reason="runtime error: invalid memory address or nil pointer dereference"

ctx.Sender().Tell(&pong{})

default:

}

})

pongPid := actor.Spawn(pongProps)

pingProps := actor.FromProducer(func() actor.Actor {

return &pingActor{

pongPid: pongPid,

}

})

pingPid := actor.Spawn(pingProps)

pingPid.Tell(struct{}{})

time.Sleep(1 * time.Second) // Just to make sure system ends after actor execution

}

[view raw](https://gist.github.com/oklahomer/7cd359f407f1cd59df7d73a20e1bafa0/raw/f2226a52a7f8e65375cb4f190bef8025300a62fe/no-sender-to-respond.go)[no-sender-to-respond.go](https://gist.github.com/oklahomer/7cd359f407f1cd59df7d73a20e1bafa0#file-no-sender-to-respond-go)  hosted with ❤ by  [GitHub](https://github.com/)

However, the recipient fails to refer to the sender actor in the same way it failed in the previous example. This may seem odd, but let us take a look at actor.Context’s implementation. A call to Context.Tell() is  [proxied to Context.sendUserMessage()](https://github.com/AsynkronIT/protoactor-go/blob/3992780c0af683deb5ec3746f4ec5845139c6e42/actor/local_context.go#L99-L101), where the message is stuffed into actor.MessageEnvelope with  [nil Sender field](https://github.com/AsynkronIT/protoactor-go/blob/3992780c0af683deb5ec3746f4ec5845139c6e42/actor/local_context.go#L120)  as below:

```go
func (ctx *localContext) Tell(pid *PID, message interface{}) {
 ctx.sendUserMessage(pid, message)
}

func (ctx *localContext) sendUserMessage(pid *PID, message interface{}) {
 if ctx.outboundMiddleware != nil {
  if env, ok := message.(*MessageEnvelope); ok {
   ctx.outboundMiddleware(ctx, pid, env)
  } else {
   ctx.outboundMiddleware(ctx, pid, &MessageEnvelope{
    Header:  nil,
    Message: message,
    Sender:  nil,
   })
  }
 } else {
  pid.ref().SendUserMessage(pid, message)
 }
}
```

That is why a recipient cannot refer to the sender even though the messaging occurs between two actors and such contextual information seems to be available. The above code fragment suggests that passing actor.MessageEnvelope with pre-filled Sender field should tell the sending actor to the recipient. This actually works because all actor.MessageEnvelope’s fields are public and accessible, but this is a cumbersome job. There should be a way to do that.

## Request() lets a recipient request for the sender reference

A second messaging method is Request(). This lets developers set who the sender actor is, and the recipient actor can reply to the sender actor by calling Context.Respond() or by calling Context.Sender().Tell(). Below is the method signature.

```go
// Request sends a messages asynchronously to the PID. The actor may send a response back via respondTo, which is
// available to the receiving actor via Context.Sender
func (pid *PID) Request(message interface{}, respondTo *PID) {
 env := &MessageEnvelope{
  Message: message,
  Header:  nil,
  Sender:  respondTo,
 }
 pid.ref().SendUserMessage(pid, env)
}
```

Above signature may look more like Akka’s ActorRef#tell than Tell() in a way that a developer can set a sender actor, more precisely a sending actor.PID in this case, as a second argument. An actor.PID and an actor.Context both have Request() method and they behave equivalently as described in the below example:

[sender-respond.go · GitHub](https://gist.github.com/oklahomer/44ef467691f4e45337b3bb742b3f5fd2)

This not only works for request-response model, but also works to propagate the sending actor identity to subsequent actor calls.

## RequestFuture() only has its future

The last method is ReqeustFuture(). This can be used as an extension of Request() where an actor.Future is returned to the requester. However, its behavior differs slightly but significantly when the recipient actor tries referring to the sender with Context.Sender() and treating this as a reference to the sender actor. Below is a simple example that demonstrates a regular request-response model:

[future.go · GitHub](https://gist.github.com/oklahomer/3b9b22ab159d473a13068cf2853a4b67)

Now the below example demonstrates how Request() and RequestFuture() behave differently when Context.Sender() or Context.Respond() is called to refer to the sender actor’s actor.PID. The code structure is almost the same as the previous example besides that below tries to send back multiple messages to the sender actor.

package main

import (

"github.com/AsynkronIT/protoactor-go/actor"

"log"

"time"

)

type pong struct {

}

type ping struct {

}

type pingActor struct {

pongPid *actor.PID

}

func (p *pingActor) Receive(ctx actor.Context) {

switch ctx.Message().(type) {

case struct{}:

// Below both work.

//

//future := p.pongPid.RequestFuture(&ping{}, time.Second)

future := ctx.RequestFuture(p.pongPid, &ping{}, time.Second)

result, err := future.Result()

if err != nil {

log.Print(err.Error())

return

}

log.Printf("Received %#v", result)

case *pong:

// Never comes here.

// When the pong actor responds to the sender,

// the sender is not a ping actor but a future process.

log.Print("Received pong message")

}

}

func main() {

pongProps := actor.FromFunc(func(ctx actor.Context) {

switch ctx.Message().(type) {

case *ping:

log.Print("Received ping message")

// Below both work in this example, but their behavior slightly differ.

// ctx.Sender().Tell() panics and recovers if the sender is nil;

// while ctx.Respond() checks the presence of sender and redirects the message to dead letter process

// when sender is absent.

//

//ctx.Sender().Tell(&pong{})

ctx.Respond(&pong{})

// Take a look at the id field.

// 2018/09/23 10:58:53 &actor.PID{Address:"nonhost", Id:"future$3", p:(*actor.Process)(0xc4200ea010)}

log.Printf("%#v", ctx.Sender())

// Below all fail because the sender PID does not represents the sender actor,

// but the sending Future process and the Future process ends when the first payload is returned.

ctx.Sender().Tell(&pong{})

ctx.Respond(&pong{})

ctx.Sender().Tell(&pong{})

ctx.Respond(&pong{})

ctx.Sender().Tell(&pong{})

ctx.Respond(&pong{})

ctx.Sender().Tell(&pong{})

ctx.Respond(&pong{})

default:

}

})

pongPid := actor.Spawn(pongProps)

pingProps := actor.FromProducer(func() actor.Actor {

return &pingActor{

pongPid: pongPid,

}

})

pingPid := actor.Spawn(pingProps)

pingPid.Tell(struct{}{})

time.Sleep(1 * time.Second) // Just to make sure system ends after actor execution

}

[view raw](https://gist.github.com/oklahomer/cc063682252369296c9684075b7b1682/raw/cea083f6733091c8071883410ae69f11816e817a/future-failure.go)[future-failure.go](https://gist.github.com/oklahomer/cc063682252369296c9684075b7b1682#file-future-failure-go)  hosted with ❤ by  [GitHub](https://github.com/)

Remember, as briefly introduced in the “Premise” section, an actor.PID not only encapsulates an actor.Actor instance but also encapsulates any actor.Process implementation. The concept of “process” and its representation, PID, are quite similar to those of Erlang in this way. With that said, let us take a closer look at how the above example behaves under the hood. First, two processes for actor PIDs are explicitly created by the developer: pingPid and pongPid. When pingPid sends a message to pongPid, another process is implicitly created by protoactor-go: that of actor.Future. And this actor.Future process is set as the sender PID when communication takes place.

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

When the recipient actor’s process, pongPid, receives the message and respond to the sender, the “sender” is not actually pingPid but the actor.Future’s process. After one message is sent back to pingPid, the actor.Future process ends and therefore the subsequent calls to Context.Respond() or Context.Sender() from pongPid fail to refer to the sender. So when the passing of sender actor’s PID is vital for the recipient’s task execution, use Request() or include the sender actor’s actor.PID in the sending message so the recipient can refer to the sender actor for sure.

## Cluster grain’s unique RPC based messaging

Actors can communicate with Cluster grains just like communicating with remote actors. In fact, protoactor-go’s cluster mechanism is implemented on top of actor.remote implementation. However, this cluster mechanism adopts the idea of Microsoft Orleans where the actor lifecycle and other major tasks are managed by the actor framework to ease the developer’s work. This effort includes the introduction of handy RPC based communication protocol. Communication with cluster grains still use Protocol Buffers for serialization and deserialization, but this goes a bit further by providing a wrapper for gRPC service calls.

By using  [gograin](https://github.com/AsynkronIT/protoactor-go/tree/dev/protobuf/protoc-gen-gograin)  protoc plugin, a code is generated for gRPC services. This code provides an actor.Actor implementation where Receive() receives a message from another actor, deserializes it and calls a corresponding method depending on the incoming message type. Developers only have to implement a method for each gRPC service. The returning value of the implemented method is returned to the sender actor. One thing to notice is that this remote gRPC call is implemented with RequestFuture() under the hood. So when the method tries referring to the sender by Context.Sender(), the returned actor.PID is not a representation of the sender actor but an actor.Future. The example contains a relatively large amount of code so visit  [my example repository](https://github.com/oklahomer/protoactor-go-sender-example/tree/master/cluster)  for details. Directory layout is as below:

-   messages … This includes messages shared by sender and recipient actors. protos_protoactor.go contains the code generated by gograin protoc plugin. This is used for the gRPC based communication.  
    
-   cluster-ping-grpc and cluster-pong-grpc … These provide implementations for ping actor and pong actor. They communicate over gRPC based protocol.  
    
-   cluster-ping-future, cluster-ping-request, cluster-ping-tell and cluster-pong … These are examples that communicate with actor.remote implementation without the gRPC service.  
    

# Conclusion

While there are several kinds of actors, those actors have unified ways to communicate with other actors no matter where they are located at. However, because an actor.PID is not only a representation of an actor process but also a representation of any actor.Process implementation, extra work may be required for a recipient actor to refer to the sender actor since the returning actor.PID of Context.Sender() is not necessarily a sender actor’s representation. To ensure that the recipient actor can refer to the sender actor, include the sender actor’s PID in the sending message or use Request(). Visit [github.com/oklahomer/protoactor-go-sender-example](https://github.com/oklahomer/protoactor-go-sender-example) for more comprehensive examples.
<!--stackedit_data:
eyJoaXN0b3J5IjpbNjU5MjM0OTA3LDY0MTQ0OTYzNiwxMDM5Nz
Y4NTA2LDg3MDMyOTE4MiwxMDM2ODcxMjc4LDIwNDgzMjQ2NjZd
fQ==
-->