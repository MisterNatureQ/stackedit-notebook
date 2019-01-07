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
    本地 actors ......那些actors  位于同一个进程中 (process并不是actor.process)
-   Remote … Actors located in different processes or servers. An actor is considered to be “local” when addressed from within the same process; while this is “remote” when addressed across a network. Because a message is sent over a network, message serialization is required. Protocol Buffers is used for this task in protoactor-go.  
    远程 actors ...位于不同进程或服务器中的Actor。  当收信地址 在同一过程中处理时，Actor被认为是“local”;  虽然这是通过网络寻址的“remote”。  由于消息是通过网络发送的，因此需要进行消息序列化。  Protocol Buffers用于protoactor-go中的此任务。
    
-   Cluster grain … A kind of remote actor but the lifecycle and other complexity are taken care of by protoactor-go library. Cluster topology is managed by  [consul](https://www.consul.io/)  and a grain can be addressed over a network. Consul manages the cluster membership and the availability of each node.  
    集群粒子 actors ......一种remote actor，但生命周期和其他复杂性由protoactor-go库来处理。  群集拓扑由[consul](https://www.consul.io/)管理，粒子可以通过网络进行寻址。  Consul管理集群成员资格和每个节点的可用性。

Thanks to the location transparency, an actor can communicate with other actors in the same way without worrying about where the recipient actors are located at. In addition to those basic communication means, a cluster grain has an extra mechanism to provide RPC based interface.

由于位置透明，演员可以以相同的方式与其他演员沟通，而不必担心收件人演员所在的位置。除了那些基本的通信手段之外，群集粒度还有一个额外的机制来提供基于RPC的接口。

Each actor is encapsulated in an actor.PID instance so developers communicate with actors via methods provided by this actor.PID. (actor.Context also provides equivalent methods, but these can be considered as wrappers for actor.PID’s corresponding methods.) One important thing to remember is that above actors are not the only entities encapsulated in actor.PIDs. As a matter of fact, any actor.Process implementation including mailbox, Future mechanism and others are also encapsulated in actor.PIDs. This may be familiar to those with Erlang background. Understanding this becomes vital when one tries referring to message sender actor. The rest of this article is going to describe each messaging method and how a recipient actor can refer to the sending actor.

每个actor都封装在一个actor.PID实例中，因此开发人员可以通过此actor.PID提供的方法与actor进行通信。  （actor.Context也提供了等效的方法，但是这些方法可以被认为是actor.PID对应方法的包装。）要记住的一件重要事情是上面的actor不是封装在actor.PID中的唯一实体。  事实上，任何actor.Process实现，包括邮箱，Future机制等也都封装在actor.PID中。  这对于具有Erlang背景的人来说可能是熟悉的。  当人们尝试引用消息发送者actor时，理解这一点变得至关重要  本文的其余部分将描述每个消息传递方法以及收件人actor如何引用发送actor。

# Communication methods 通信方法

Below are the common communication methods – Tell(), Request() and RequestFuture() – and RPC based method for cluster grain. Examples in this article all demonstrate local actor messaging because local and remote actors share a common messaging interface. Visit my  [example repository](https://github.com/oklahomer/protoactor-go-sender-example)  to cover all messaging implementations of local, remote and cluster grain.

下面是常见的通信方法 - Tell()，Request()和RequestFuture() - 以及基于RPC的集群粒度方法。  本文中的示例都演示了本地actor消息传递，因为本地和远程actor共享一个公共消息传递接口。  访问[示例存储库](https://github.com/oklahomer/protoactor-go-sender-example)以涵盖本地，远程和集群粒度的所有消息传递实现。

## Tell() tells nothing about the sender 没有告知发件人
To send a message to an actor, one may call actor.PID’s Tell() method. When a message is sent from outside of an actor system by calling PID.Tell(), the recipient actor fails to refer to the sending actor with [Context.Sender()](https://github.com/AsynkronIT/protoactor-go/blob/3992780c0af683deb5ec3746f4ec5845139c6e42/actor/context.go#L16-L17). This is pretty obvious. Because the message is sent from outside, there is no such thing as sending actor. Below is an example:

要向actor发送消息，可以调用actor.PID的Tell（）方法。  当通过调用PID.Tell（）从actor系统外部发送消息时，接收方actor无法使用[Context.Sender（）](https://github.com/AsynkronIT/protoactor-go/blob/3992780c0af683deb5ec3746f4ec5845139c6e42/actor/context.go#L16-L17)引用发送actor。  这很明显。  因为消息是从外部发送的，所以没有发送actor这样的东西。  以下是一个例子：

```go
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
```
[view raw](https://gist.github.com/oklahomer/2cd42e0c19abf3d10b671f5a98e71571/raw/dafa0c3635da253a3497b614fd215f8a6ee6a052/no-sender.go)[no-sender.go](https://gist.github.com/oklahomer/2cd42e0c19abf3d10b671f5a98e71571#file-no-sender-go)  hosted with ❤ by  [GitHub](https://github.com/)


In the above example, a message is directly sent to an actor from outside of an actor system. Therefore the recipient actor fails to refer to the sending actor. With Akka, this behavior is similar to set [ActorRef#noSender](https://doc.akka.io/japi/akka/2.4.2/akka/actor/ActorRef.html#noSender--) as the second argument of [ActorRef#tell](https://doc.akka.io/japi/akka/2.4.2/akka/actor/ActorRef.html#tell-java.lang.Object-akka.actor.ActorRef-) – when the recipient tries to respond, the message goes to the dead letter mailbox.

在上面的示例中，消息直接从actor系统外部发送给actor。  因此，接收方actor无法引用发送方。  对于Akka，此行为类似于将[ActorRef＃noSender](https://doc.akka.io/japi/akka/2.4.2/akka/actor/ActorRef.html#noSender--) 设置为[ActorRef＃tell](https://doc.akka.io/japi/akka/2.4.2/akka/actor/ActorRef.html#tell-java.lang.Object-akka.actor.ActorRef-) 的第二个参数 - 当收件人尝试响应时，邮件将转到死信邮箱。

When a message is sent from one actor to another, there indeed is a sender-recipient relationship. Recipient actor’s contextual information, actor.Context, appears to provide such information for us. Below is an example code that tries to refer to the sender actor with actor.Context:

当消息从一个actor发送到另一个actor时，确实存在发送者 - 接收者关系。  收件人actor的上下文信息，actor.Context，似乎为我们提供了这样的信息。  下面是一个示例代码，它尝试使用actor.Context引用发送方actor：

```go
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
		// 下面没有为ctx.Self() 设置为 sender，即使消息是通过actor.context从另一个actor发送的 接收者不知道发送者。
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
			// 尝试通过 内部 context 的 Sender() 来回复
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
```
[view raw](https://gist.github.com/oklahomer/7cd359f407f1cd59df7d73a20e1bafa0/raw/f2226a52a7f8e65375cb4f190bef8025300a62fe/no-sender-to-respond.go)[no-sender-to-respond.go](https://gist.github.com/oklahomer/7cd359f407f1cd59df7d73a20e1bafa0#file-no-sender-to-respond-go)  hosted with ❤ by  [GitHub](https://github.com/)

However, the recipient fails to refer to the sender actor in the same way it failed in the previous example. This may seem odd, but let us take a look at actor.Context’s implementation. A call to Context.Tell() is  [proxied to Context.sendUserMessage()](https://github.com/AsynkronIT/protoactor-go/blob/3992780c0af683deb5ec3746f4ec5845139c6e42/actor/local_context.go#L99-L101), where the message is stuffed into actor.MessageEnvelope with  [nil Sender field](https://github.com/AsynkronIT/protoactor-go/blob/3992780c0af683deb5ec3746f4ec5845139c6e42/actor/local_context.go#L120)  as below:

但是，收件人无法以与上一示例中失败相同的方式引用发送方actor。  这可能看起来很奇怪，但让我们来看看actor.Context的实现。  对Context.Tell（）的调用[代理到Context.sendUserMessage（）](https://github.com/AsynkronIT/protoactor-go/blob/3992780c0af683deb5ec3746f4ec5845139c6e42/actor/local_context.go#L99-L101)  ，其中消息被填充到带有[nil Sender字段的](https://github.com/AsynkronIT/protoactor-go/blob/3992780c0af683deb5ec3746f4ec5845139c6e42/actor/local_context.go#L120)  actor.MessageEnvelope中，如下所示：

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
	    // 消息被填充 nil 字段
	    Sender:  nil,
   })
  }
 } else {
  pid.ref().SendUserMessage(pid, message)
 }
}
```

That is why a recipient cannot refer to the sender even though the messaging occurs between two actors and such contextual information seems to be available. The above code fragment suggests that passing actor.MessageEnvelope with pre-filled Sender field should tell the sending actor to the recipient. This actually works because all actor.MessageEnvelope’s fields are public and accessible, but this is a cumbersome job. There should be a way to do that.

这就是为什么即使在两个 actors  之间发生消息传递并且这样的上下文信息似乎可用，接收者也不能引用发送者。  上面的代码片段表明，传递带有预填充发件人字段的actor.MessageEnvelope应该告诉发送者接收者。  这实际上是有效的，因为所有actor.MessageEnvelope的字段都是公共的和可访问的，但这是一项繁琐的工作。  应该有办法做到这一点。

## Request() lets a recipient request for the sender reference  使收件人可以请求引用发件人

A second messaging method is Request(). This lets developers set who the sender actor is, and the recipient actor can reply to the sender actor by calling Context.Respond() or by calling Context.Sender().Tell(). Below is the method signature.
第二种消息传递方法是Request（）。  这允许开发人员设置发送者actor的身份，接收者actor可以通过调用Context.Respond（）或通过调用Context.Sender（）来回复发送者actor.Tell（）。  以下是方法签名。
```go
// Request sends a messages asynchronously to the PID. The actor may send a response back via respondTo, which is available to the receiving actor via Context.Sender
// 请求异步地向PID发送消息。actor 可以通过respondTo发送响应，接收参与者可以通过Context.Sender进行响应
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

上面的签名可能看起来更像Akka的ActorRef＃tell而不是Tell（），开发人员可以设置发送方actor，更确切地说是发送actor.PID，在这种情况下，作为第二个参数。  actor.PID和actor.Context都有Request（）方法，它们的行为与以下示例中描述的相同：

[sender-respond.go · GitHub](https://gist.github.com/oklahomer/44ef467691f4e45337b3bb742b3f5fd2)

This not only works for request-response model, but also works to propagate the sending actor identity to subsequent actor calls.
这不仅适用于请求 - 响应模型，还可以将发送方标识传播到后续的actor调用。

## RequestFuture() only has its future

The last method is ReqeustFuture(). This can be used as an extension of Request() where an actor.Future is returned to the requester. However, its behavior differs slightly but significantly when the recipient actor tries referring to the sender with Context.Sender() and treating this as a reference to the sender actor. Below is a simple example that demonstrates a regular request-response model:
最后一个方法是ReqeustFuture（）。  这可以用作Request（）的扩展，其中actor.Future返回给请求者。  但是，，其行为略有不同, 但 当接收方actor尝试使用Context.Sender（）引用发送方并将其视为对发送方actor的引用时 显著不同。  下面是一个演示常规请求 - 响应模型的简单示例：

[future.go · GitHub](https://gist.github.com/oklahomer/3b9b22ab159d473a13068cf2853a4b67)

Now the below example demonstrates how Request() and RequestFuture() behave differently when Context.Sender() or Context.Respond() is called to refer to the sender actor’s actor.PID. The code structure is almost the same as the previous example besides that below tries to send back multiple messages to the sender actor.

现在，下面的示例演示了当调用Context.Sender（）或Context.Respond（）来引用发送方actor的actor.PID时，Request（）和RequestFuture（）的行为方式不同。  代码结构与前面的示例几乎相同，除了以下尝试将多个消息发送回发送方actor。

```go
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

		// 创建future 等待结果
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
				// 下面这两种方法在本例中都有效，但是它们的行为略有不同。
				// 如果发送方为nil，则 ctx.Sender().Tell() panics  并 recovers  ;
				// 而ctx. response()检查发送方的存在，并将消息重定向到死信进程
				//ctx.Sender().Tell(&pong{})
				ctx.Respond(&pong{})

				// Take a look at the id field.
				// 2018/09/23 10:58:53 &actor.PID{Address:"nonhost", Id:"future$3", p:(*actor.Process)(0xc4200ea010)}
				log.Printf("%#v", ctx.Sender())

				// Below all fail because the sender PID does not represents the sender actor,
				// but the sending Future process and the Future process ends when the first payload is returned.
				// 下面全部失败，因为发送方PID不代表发送方参与者，
				// 但是发送 Future process 和 Future process 在返回第一个有效负载时结束。
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
	
	// 响应方
	pongPid := actor.Spawn(pongProps)
	
	pingProps := actor.FromProducer(func() actor.Actor {
		return &pingActor{
			pongPid: pongPid,
		}
	})
	
	// 发送方
	pingPid := actor.Spawn(pingProps)
	pingPid.Tell(struct{}{})
	time.Sleep(1 * time.Second) // Just to make sure system ends after actor execution
}
```
[view raw](https://gist.github.com/oklahomer/cc063682252369296c9684075b7b1682/raw/cea083f6733091c8071883410ae69f11816e817a/future-failure.go)[future-failure.go](https://gist.github.com/oklahomer/cc063682252369296c9684075b7b1682#file-future-failure-go)  hosted with ❤ by  [GitHub](https://github.com/)

Remember, as briefly introduced in the “Premise” section, an actor.PID not only encapsulates an actor.Actor instance but also encapsulates any actor.Process implementation. The concept of “process” and its representation, PID, are quite similar to those of Erlang in this way. With that said, let us take a closer look at how the above example behaves under the hood. First, two processes for actor PIDs are explicitly created by the developer: pingPid and pongPid. When pingPid sends a message to pongPid, another process is implicitly created by protoactor-go: that of actor.Future. And this actor.Future process is set as the sender PID when communication takes place.

请记住，正如“前提”部分中简要介绍的那样，actor.PID不仅封装了actor.Actor实例，还封装了任何actor.Process实现。  “process”的概念及其表示PID与Erlang的概念非常类似。  话虽如此，让我们仔细看看上面的例子如何在幕后表现。  首先，开发人员明确创建了两个actor PID进程：pingPid和pongPid。  当pingPid向pongPid发送消息时，protoactor-go隐式创建另一个process ：actor.Future。  并且此actor.Future进程在进行通信时设置为发送方PID。

```go
func (ctx *localContext) RequestFuture(pid *PID, message interface{}, timeout time.Duration) *Future {
 future := NewFuture(timeout)
 env := &MessageEnvelope{
  Header:  nil,
  Message: message,
  // 注意这里填充的 是 future 的PID
  Sender:  future.PID(),
 }
 ctx.sendUserMessage(pid, env)

 return future
}
```

When the recipient actor’s process, pongPid, receives the message and respond to the sender, the “sender” is not actually pingPid but the actor.Future’s process. After one message is sent back to pingPid, the actor.Future process ends and therefore the subsequent calls to Context.Respond() or Context.Sender() from pongPid fail to refer to the sender. So when the passing of sender actor’s PID is vital for the recipient’s task execution, use Request() or include the sender actor’s actor.PID in the sending message so the recipient can refer to the sender actor for sure.

**当收件人actor的进程pongPid收到邮件并回复发件人时，“sender”实际上并不是pingPid而是actor.Future的进程。  将一条消息发送回pingPid后，actor.Future进程结束，因此从pongPid对Context.Respond（）或Context.Sender（）的后续调用无法引用发送方。 因此，当发送方actor的PID的传递对于接收方的任务执行至关重要时，请使用Request（）或在发送消息中包含发送方actor的actor.PID，以便接收方可以确定地引用发送方actor 。**

## Cluster grain’s unique RPC based messaging   Cluster grain’独特的基于RPC的消息传递

Actors can communicate with Cluster grains just like communicating with remote actors. In fact, protoactor-go’s cluster mechanism is implemented on top of actor.remote implementation. However, this cluster mechanism adopts the idea of Microsoft Orleans where the actor lifecycle and other major tasks are managed by the actor framework to ease the developer’s work. This effort includes the introduction of handy RPC based communication protocol. Communication with cluster grains still use Protocol Buffers for serialization and deserialization, but this goes a bit further by providing a wrapper for gRPC service calls.
Actors  可以与群集粒子进行通信，就像与远程角色进行通信一样。  事实上，protoactor-go的集群机制是在actor.remote实现之上实现的。  但是，这种集群机制采用了Microsoft Orleans的思想，其中actor的生命周期和其他主要任务由actor框架管理，以简化开发人员的工作。  这项工作包括引入方便的基于RPC的通信协议。  与群集粒子的通信仍然使用  Protocol Buffers 进行序列化和反序列化，但是通过为gRPC服务调用提供包装器可以进一步实现这一点。

By using  [gograin](https://github.com/AsynkronIT/protoactor-go/tree/dev/protobuf/protoc-gen-gograin)  protoc plugin, a code is generated for gRPC services. This code provides an actor.Actor implementation where Receive() receives a message from another actor, deserializes it and calls a corresponding method depending on the incoming message type. Developers only have to implement a method for each gRPC service. The returning value of the implemented method is returned to the sender actor. One thing to notice is that this remote gRPC call is implemented with RequestFuture() under the hood. So when the method tries referring to the sender by Context.Sender(), the returned actor.PID is not a representation of the sender actor but an actor.Future. The example contains a relatively large amount of code so visit  [my example repository](https://github.com/oklahomer/protoactor-go-sender-example/tree/master/cluster)  for details. Directory layout is as below:

通过使用[gograin protoc](https://github.com/AsynkronIT/protoactor-go/tree/dev/protobuf/protoc-gen-gograin)protoc插件，为gRPC服务生成代码。  此代码提供了一个actor.Actor实现，其中Receive（）从另一个actor接收消息，反序列化它并根据传入的消息类型调用相应的方法。  开发人员只需为每个gRPC服务实现一种方法。  已实现方法的返回值将返回给发送方actor。  **需要注意的一点是，这个远程gRPC调用是通过引擎下的RequestFuture（）实现的。  因此，当方法尝试通过Context.Sender（）引用发送方时，返回的actor.PID不是发送方actor的表示，而是actor.Future。 **  该示例包含相对大量的代码，因此请访问[我的示例存储库](https://github.com/oklahomer/protoactor-go-sender-example/tree/master/cluster) 以获取详细信息。  目录布局如下：

-   messages … This includes messages shared by sender and recipient actors. protos_protoactor.go contains the code generated by gograin protoc plugin. This is used for the gRPC based communication.  
-   cluster-ping-grpc and cluster-pong-grpc … These provide implementations for ping actor and pong actor. They communicate over gRPC based protocol.  
-   cluster-ping-future, cluster-ping-request, cluster-ping-tell and cluster-pong … These are examples that communicate with actor.remote implementation without the gRPC service.  
    
-   消息...这包括发件人和收件人actors共享的消息。  protos_protoactor.go包含由gograin protoc插件生成的代码。  这用于基于gRPC的通信。  
-   cluster-ping-grpc和cluster-pong-grpc ...这些提供了ping actor和pong actor的实现。  它们通过基于gRPC的协议进行通信。  
-   cluster-ping-future，cluster-ping-request，cluster-ping-tell和cluster-pong ......这些是在没有gRPC服务的情况下与actor.remote实现通信的示例。

# Conclusion

While there are several kinds of actors, those actors have unified ways to communicate with other actors no matter where they are located at. However, because an actor.PID is not only a representation of an actor process but also a representation of any actor.Process implementation, extra work may be required for a recipient actor to refer to the sender actor since the returning actor.PID of Context.Sender() is not necessarily a sender actor’s representation. To ensure that the recipient actor can refer to the sender actor, include the sender actor’s PID in the sending message or use Request(). Visit [github.com/oklahomer/protoactor-go-sender-example](https://github.com/oklahomer/protoactor-go-sender-example) for more comprehensive examples.

虽然有几种演员，但这些演员有统一的方式与其他演员沟通，无论他们身在何处。  但是，因为actor.PID不仅是actor进程的表示，而且是任何actor.Process实现的表示，因为返回的actor的actor.PID，接收方actor可能需要额外的工作来引用发送方actor。 .Sender（）不一定是发送者角色的表示。  要确保收件人actor可以引用发送方actor，请在发送消息中包含发送方actor的PID或使用Request（）。  访问[github.com/oklahomer/protoactor-go-sender-example](https://github.com/oklahomer/protoactor-go-sender-example)以获取更全面的示例。






# [Golang] protoactor-go 101：演员如何相互沟通

设计基于演员的程序就是将任务分成更小的部分。  细粒度的演员专注于他们的任务，与其他演员合作，完成一项重大任务。  因此，掌握演员的沟通机制和对定义明确的消息进行建模始终是设计演员系统的关键。  本文描述了protoactor-go的actor类别，它们的消息传递方法以及这些方法在引用发送方actor方面的不同之处。

请参阅我之前的文章，  [[Golang] protoactor-go 101：介绍golang的actor模型实现](https://translate.googleusercontent.com/translate_c?depth=1&hl=zh-CN&prev=search&rurl=translate.google.com&sl=en&sp=nmt4&u=https://blog.oklahome.net/2018/07/protoactor-go-introduction.html&xid=17259,15700023,15700124,15700186,15700191,15700201,15700237,15700242,15700248&usg=ALkJrhh3LnHnZU_HYyH9M659eEmy_10RFw)  ，了解protoactor-go的基本概念和术语。

-   [TL; DR](https://translate.googleusercontent.com/translate_c?depth=1&hl=zh-CN&prev=search&rurl=translate.google.com&sl=en&sp=nmt4&u=https://blog.oklahome.net/2018/09/protoactor-go-messaging-protocol.html&xid=17259,15700023,15700124,15700186,15700191,15700201,15700237,15700242,15700248&usg=ALkJrhi8UkvQn3XZbbVsJ0UES-xFZBwRQQ#toc_0)
-   [示例代码](https://translate.googleusercontent.com/translate_c?depth=1&hl=zh-CN&prev=search&rurl=translate.google.com&sl=en&sp=nmt4&u=https://blog.oklahome.net/2018/09/protoactor-go-messaging-protocol.html&xid=17259,15700023,15700124,15700186,15700191,15700201,15700237,15700242,15700248&usg=ALkJrhi8UkvQn3XZbbVsJ0UES-xFZBwRQQ#toc_1)
-   [前提：三大类演员](https://translate.googleusercontent.com/translate_c?depth=1&hl=zh-CN&prev=search&rurl=translate.google.com&sl=en&sp=nmt4&u=https://blog.oklahome.net/2018/09/protoactor-go-messaging-protocol.html&xid=17259,15700023,15700124,15700186,15700191,15700201,15700237,15700242,15700248&usg=ALkJrhi8UkvQn3XZbbVsJ0UES-xFZBwRQQ#toc_2)
-   [沟通方式](https://translate.googleusercontent.com/translate_c?depth=1&hl=zh-CN&prev=search&rurl=translate.google.com&sl=en&sp=nmt4&u=https://blog.oklahome.net/2018/09/protoactor-go-messaging-protocol.html&xid=17259,15700023,15700124,15700186,15700191,15700201,15700237,15700242,15700248&usg=ALkJrhi8UkvQn3XZbbVsJ0UES-xFZBwRQQ#toc_3)
    -   [Tell（）没有告诉发件人](https://translate.googleusercontent.com/translate_c?depth=1&hl=zh-CN&prev=search&rurl=translate.google.com&sl=en&sp=nmt4&u=https://blog.oklahome.net/2018/09/protoactor-go-messaging-protocol.html&xid=17259,15700023,15700124,15700186,15700191,15700201,15700237,15700242,15700248&usg=ALkJrhi8UkvQn3XZbbVsJ0UES-xFZBwRQQ#toc_4)
    -   [Request（）允许收件人请求发件人引用](https://translate.googleusercontent.com/translate_c?depth=1&hl=zh-CN&prev=search&rurl=translate.google.com&sl=en&sp=nmt4&u=https://blog.oklahome.net/2018/09/protoactor-go-messaging-protocol.html&xid=17259,15700023,15700124,15700186,15700191,15700201,15700237,15700242,15700248&usg=ALkJrhi8UkvQn3XZbbVsJ0UES-xFZBwRQQ#toc_5)
    -   [RequestFuture（）只有它的未来](https://translate.googleusercontent.com/translate_c?depth=1&hl=zh-CN&prev=search&rurl=translate.google.com&sl=en&sp=nmt4&u=https://blog.oklahome.net/2018/09/protoactor-go-messaging-protocol.html&xid=17259,15700023,15700124,15700186,15700191,15700201,15700237,15700242,15700248&usg=ALkJrhi8UkvQn3XZbbVsJ0UES-xFZBwRQQ#toc_6)
    -   [Cluster grain独特的基于RPC的消息传递](https://translate.googleusercontent.com/translate_c?depth=1&hl=zh-CN&prev=search&rurl=translate.google.com&sl=en&sp=nmt4&u=https://blog.oklahome.net/2018/09/protoactor-go-messaging-protocol.html&xid=17259,15700023,15700124,15700186,15700191,15700201,15700237,15700242,15700248&usg=ALkJrhi8UkvQn3XZbbVsJ0UES-xFZBwRQQ#toc_7)
-   [结论](https://translate.googleusercontent.com/translate_c?depth=1&hl=zh-CN&prev=search&rurl=translate.google.com&sl=en&sp=nmt4&u=https://blog.oklahome.net/2018/09/protoactor-go-messaging-protocol.html&xid=17259,15700023,15700124,15700186,15700191,15700201,15700237,15700242,15700248&usg=ALkJrhi8UkvQn3XZbbVsJ0UES-xFZBwRQQ#toc_8)

# TL; DR

虽然有几种演员，但这些演员共享一个统一的界面来相互沟通。  为它们的通信提供了各种方法，但总是使用Request（）来确认发送方actor所在的接收方actor。  如果这不是一个选项，  [请](https://github.com/AsynkronIT/protoactor-go/blob/3992780c0af683deb5ec3746f4ec5845139c6e42/actor/pid.go#L12-L17)在发送消息中包含发送方actor的[actor.PID](https://github.com/AsynkronIT/protoactor-go/blob/3992780c0af683deb5ec3746f4ec5845139c6e42/actor/pid.go#L12-L17)  。

# 示例代码

覆盖所有actor实现的所有通信手段的示例代码位于[github.com/oklahomer/protoactor-go-sender-example](https://translate.googleusercontent.com/translate_c?depth=1&hl=zh-CN&prev=search&rurl=translate.google.com&sl=en&sp=nmt4&u=https://github.com/oklahomer/protoactor-go-sender-example&xid=17259,15700023,15700124,15700186,15700191,15700201,15700237,15700242,15700248&usg=ALkJrhg0UJk0c1Kf4lY6TIgkjG3UdWQmsg)  。  本文中介绍了最小的示例以供说明，但请访问此存储库以获取全面的示例。

# 前提：三大类演员

protoactor-go有三种演员：本地，远程和集群。

[![](https://3.bp.blogspot.com/-Rn0COjNd0qQ/W6hy96hicDI/AAAAAAAAA2U/HjaHhdsEOLAYt3BrQT8GXFkKnjuxUwNlgCEwYBhgL/s1600/components.png)](https://3.bp.blogspot.com/-Rn0COjNd0qQ/W6hy96hicDI/AAAAAAAAA2U/HjaHhdsEOLAYt3BrQT8GXFkKnjuxUwNlgCEwYBhgL/s1600/components.png)

-   本地......那些演员位于同一个过程中。  
    
-   远程...位于不同进程或服务器中的Actor。  当在同一过程中处理时，行为者被认为是“本地的”;  虽然这是通过网络寻址的“远程”。  由于消息是通过网络发送的，因此需要进行消息序列化。  协议缓冲区用于protoactor-go中的此任务。  
    
-   集群粒子......一种远程演员，但生命周期和其他复杂性由protoactor-go库来处理。  群集拓扑由[consul](https://translate.googleusercontent.com/translate_c?depth=1&hl=zh-CN&prev=search&rurl=translate.google.com&sl=en&sp=nmt4&u=https://www.consul.io/&xid=17259,15700023,15700124,15700186,15700191,15700201,15700237,15700242,15700248&usg=ALkJrhg6L0vDyIWZfgiAHiZM7hqPzccqGw)管理，粮食可以通过网络进行寻址。  Consul管理集群成员资格和每个节点的可用性。  
    

由于位置透明，演员可以以相同的方式与其他演员沟通，而不必担心收件人演员所在的位置。除了那些基本的通信手段之外，群集粒度还有一个额外的机制来提供基于RPC的接口。

每个actor都封装在一个actor.PID实例中，因此开发人员可以通过此actor.PID提供的方法与actor进行通信。  （actor.Context也提供了等效的方法，但是这些方法可以被认为是actor.PID对应方法的包装。）要记住的一件重要事情是上面的actor不是封装在actor.PID中的唯一实体。  事实上，任何actor.Process实现，包括邮箱，Future机制等也都封装在actor.PID中。  这对于具有Erlang背景的人来说可能是熟悉的。  当人们尝试引用消息发送者actor时，理解这一点变得至关重要  本文的其余部分将描述每个消息传递方法以及收件人actor如何引用发送actor。

# 沟通方式

下面是常见的通信方法 - Tell（），Request（）和RequestFuture（） - 以及基于RPC的集群粒度方法。  本文中的示例都演示了本地actor消息传递，因为本地和远程actor共享一个公共消息传递接口。  访问我的[示例存储库，](https://translate.googleusercontent.com/translate_c?depth=1&hl=zh-CN&prev=search&rurl=translate.google.com&sl=en&sp=nmt4&u=https://github.com/oklahomer/protoactor-go-sender-example&xid=17259,15700023,15700124,15700186,15700191,15700201,15700237,15700242,15700248&usg=ALkJrhg0UJk0c1Kf4lY6TIgkjG3UdWQmsg)以涵盖本地，远程和集群粒度的所有消息传递实现。

## Tell（）没有告诉发件人

要向actor发送消息，可以调用actor.PID的Tell（）方法。  当通过调用PID.Tell（）从actor系统外部发送消息时，接收方actor无法使用[Context.Sender（）](https://github.com/AsynkronIT/protoactor-go/blob/3992780c0af683deb5ec3746f4ec5845139c6e42/actor/context.go#L16-L17)引用发送actor。  这很明显。  因为消息是从外部发送的，所以没有发送actor这样的东西。  以下是一个例子：

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

在上面的示例中，消息直接从actor系统外部发送给actor。  因此，接收方actor无法引用发送方。  对于Akka，此行为类似于将[ActorRef＃noSender](https://translate.googleusercontent.com/translate_c?depth=1&hl=zh-CN&prev=search&rurl=translate.google.com&sl=en&sp=nmt4&u=https://doc.akka.io/japi/akka/2.4.2/akka/actor/ActorRef.html&xid=17259,15700023,15700124,15700186,15700191,15700201,15700237,15700242,15700248&usg=ALkJrhhGpY-GzvqMy6K6UtNSEly_E7neLw#noSender--)设置为[ActorRef＃tell](https://translate.googleusercontent.com/translate_c?depth=1&hl=zh-CN&prev=search&rurl=translate.google.com&sl=en&sp=nmt4&u=https://doc.akka.io/japi/akka/2.4.2/akka/actor/ActorRef.html&xid=17259,15700023,15700124,15700186,15700191,15700201,15700237,15700242,15700248&usg=ALkJrhhGpY-GzvqMy6K6UtNSEly_E7neLw#noSender--)的第二个参数 - 当收件人尝试响应时，邮件将转到死信邮箱。

当消息从一个actor发送到另一个actor时，确实存在发送者 - 接收者关系。  收件人actor的上下文信息，actor.Context，似乎为我们提供了这样的信息。  下面是一个示例代码，它尝试使用actor.Context引用发送方actor：

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

但是，收件人无法以与上一示例中失败相同的方式引用发送方actor。  这可能看起来很奇怪，但让我们来看看actor.Context的实现。  对Context.Tell（）的调用[代理到Context.sendUserMessage（）](https://github.com/AsynkronIT/protoactor-go/blob/3992780c0af683deb5ec3746f4ec5845139c6e42/actor/local_context.go#L99-L101)  ，其中消息被填充到带有[nil Sender字段的](https://github.com/AsynkronIT/protoactor-go/blob/3992780c0af683deb5ec3746f4ec5845139c6e42/actor/local_context.go#L120)  actor.MessageEnvelope中，如下所示：

```go
func (ctx *localContext) Tell(pid *PID, message interface{}) { ctx.sendUserMessage(pid, message) } func (ctx *localContext) sendUserMessage(pid *PID, message interface{}) { if ctx.outboundMiddleware != nil { if env, ok := message.(*MessageEnvelope); ok { ctx.outboundMiddleware(ctx, pid, env) } else { ctx.outboundMiddleware(ctx, pid, &MessageEnvelope{ Header: nil, Message: message, Sender: nil, }) } } else { pid.ref().SendUserMessage(pid, message) } }
```

这就是为什么即使在两个演员之间发生消息传递并且这样的上下文信息似乎可用，接收者也不能引用发送者。  上面的代码片段表明，传递带有预填充发件人字段的actor.MessageEnvelope应该告诉发送者接收者。  这实际上是有效的，因为所有actor.MessageEnvelope的字段都是公共的和可访问的，但这是一项繁琐的工作。  应该有办法做到这一点。

## Request（）允许收件人请求发件人引用

第二种消息传递方法是Request（）。  这允许开发人员设置发送者actor的身份，接收者actor可以通过调用Context.Respond（）或通过调用Context.Sender（）来回复发送者actor.Tell（）。  以下是方法签名。

 `// Request sends a messages asynchronously to the PID. The actor may send a response back via respondTo, which is // available to the receiving actor via Context.Sender func (pid *PID) Request(message interface{}, respondTo *PID) { env := &MessageEnvelope{ Message: message, Header: nil, Sender: respondTo, } pid.ref().SendUserMessage(pid, env) }` 

上面的签名可能看起来更像Akka的ActorRef＃tell而不是Tell（），开发人员可以设置发送方actor，更确切地说是发送actor.PID，在这种情况下，作为第二个参数。  actor.PID和actor.Context都有Request（）方法，它们的行为与以下示例中描述的相同：

[sender-respond.go·GitHub](https://translate.googleusercontent.com/translate_c?depth=1&hl=zh-CN&prev=search&rurl=translate.google.com&sl=en&sp=nmt4&u=https://gist.github.com/oklahomer/44ef467691f4e45337b3bb742b3f5fd2&xid=17259,15700023,15700124,15700186,15700191,15700201,15700237,15700242,15700248&usg=ALkJrhhviXwx3_t92vCJlZnbT2U76PGkQA)

这不仅适用于请求 - 响应模型，还可以将发送方标识传播到后续的actor调用。

## RequestFuture（）只有它的未来

最后一个方法是ReqeustFuture（）。  这可以用作Request（）的扩展，其中actor.Future返回给请求者。  但是，当接收方actor尝试使用Context.Sender（）引用发送方并将其视为对发送方actor的引用时，其行为略有不同但显着不同。  下面是一个演示常规请求 - 响应模型的简单示例：

[future.go·GitHub](https://translate.googleusercontent.com/translate_c?depth=1&hl=zh-CN&prev=search&rurl=translate.google.com&sl=en&sp=nmt4&u=https://gist.github.com/oklahomer/3b9b22ab159d473a13068cf2853a4b67&xid=17259,15700023,15700124,15700186,15700191,15700201,15700237,15700242,15700248&usg=ALkJrhhLxgS__QdtMpV_f27QzpjXDgp63A)

现在，下面的示例演示了当调用Context.Sender（）或Context.Respond（）来引用发送方actor的actor.PID时，Request（）和RequestFuture（）的行为方式不同。  代码结构与前面的示例几乎相同，除了以下尝试将多个消息发送回发送方actor。

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

请记住，正如“前提”部分中简要介绍的那样，actor.PID不仅封装了actor.Actor实例，还封装了任何actor.Process实现。  “过程”的概念及其表示PID与Erlang的概念非常类似。  话虽如此，让我们仔细看看上面的例子如何在幕后表现。  首先，开发人员明确创建了两个actor PID进程：pingPid和pongPid。  当pingPid向pongPid发送消息时，protoactor-go隐式创建另一个进程：actor.Future。  并且此actor.Future进程在进行通信时设置为发送方PID。

 `func (ctx *localContext)  RequestFuture(pid *PID, message interface{}, timeout time.Duration)  *Future  { future :=  NewFuture(timeout) env :=  &MessageEnvelope{  Header:  nil,  Message: message,  Sender: future.PID(),  } ctx.sendUserMessage(pid, env)  return future }` 

当收件人actor的进程pongPid收到邮件并回复发件人时，“sender”实际上并不是pingPid而是actor.Future的进程。  将一条消息发送回pingPid后，actor.Future进程结束，因此从pongPid对Context.Respond（）或Context.Sender（）的后续调用无法引用发送方。  因此，当发送方actor的PID的传递对于接收方的任务执行至关重要时，请使用Request（）或在发送消息中包含发送方actor的actor.PID，以便接收方可以确定地引用发送方actor。

## Cluster grain独特的基于RPC的消息传递

演员可以与群集粒子进行通信，就像与远程角色进行通信一样。  事实上，protoactor-go的集群机制是在actor.remote实现之上实现的。  但是，这种集群机制采用了Microsoft Orleans的思想，其中actor的生命周期和其他主要任务由actor框架管理，以简化开发人员的工作。  这项工作包括引入方便的基于RPC的通信协议。  与簇粒子的通信仍然使用协议缓冲器进行序列化和反序列化，但是通过为gRPC服务调用提供包装器可以进一步实现这一点。

通过使用[gograin protoc](https://translate.googleusercontent.com/translate_c?depth=1&hl=zh-CN&prev=search&rurl=translate.google.com&sl=en&sp=nmt4&u=https://github.com/AsynkronIT/protoactor-go/tree/dev/protobuf/protoc-gen-gograin&xid=17259,15700023,15700124,15700186,15700191,15700201,15700237,15700242,15700248&usg=ALkJrhhOem_CeS7yv2BtD3fTiXuFIw_-Sw)插件，为gRPC服务生成代码。  此代码提供了一个actor.Actor实现，其中Receive（）从另一个actor接收消息，反序列化它并根据传入的消息类型调用相应的方法。  开发人员只需为每个gRPC服务实现一种方法。  已实现方法的返回值将返回给发送方actor。  需要注意的一点是，这个远程gRPC调用是通过引擎下的RequestFuture（）实现的。  因此，当方法尝试通过Context.Sender（）引用发送方时，返回的actor.PID不是发送方actor的表示，而是actor.Future。  该示例包含相对大量的代码，因此请访问[我的示例存储库](https://translate.googleusercontent.com/translate_c?depth=1&hl=zh-CN&prev=search&rurl=translate.google.com&sl=en&sp=nmt4&u=https://github.com/oklahomer/protoactor-go-sender-example/tree/master/cluster&xid=17259,15700023,15700124,15700186,15700191,15700201,15700237,15700242,15700248&usg=ALkJrhjFz3lvN28NLx1sFl9qNCcBIH2How)以获取详细信息。  目录布局如下：

-   消息...这包括发件人和收件人参与者共享的消息。  protos_protoactor.go包含由gograin protoc插件生成的代码。  这用于基于gRPC的通信。  
    
-   cluster-ping-grpc和cluster-pong-grpc ...这些提供了ping actor和pong actor的实现。  它们通过基于gRPC的协议进行通信。  
    
-   cluster-ping-future，cluster-ping-request，cluster-ping-tell和cluster-pong ......这些是在没有gRPC服务的情况下与actor.remote实现通信的示例。  
    

# 结论

虽然有几种演员，但这些演员有统一的方式与其他演员沟通，无论他们身在何处。  但是，因为actor.PID不仅是actor进程的表示，而且是任何actor.Process实现的表示，因为返回的actor的actor.PID，接收方actor可能需要额外的工作来引用发送方actor。 .Sender（）不一定是发送者角色的表示。  要确保收件人actor可以引用发送方actor，请在发送消息中包含发送方actor的PID或使用Request（）。  访问[github.com/oklahomer/protoactor-go-sender-example](https://translate.googleusercontent.com/translate_c?depth=1&hl=zh-CN&prev=search&rurl=translate.google.com&sl=en&sp=nmt4&u=https://github.com/oklahomer/protoactor-go-sender-example&xid=17259,15700023,15700124,15700186,15700191,15700201,15700237,15700242,15700248&usg=ALkJrhg0UJk0c1Kf4lY6TIgkjG3UdWQmsg)以获取更全面的示例。
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTcyNzQyNzQ5NywtNjAzOTE3OTQsLTE0MT
k0Njg4ODAsMTAxNzAzNTUwNCwxNTI0NTM2MTYyLDE3MTc2MDQw
OTgsNjU5MjM0OTA3LDY0MTQ0OTYzNiwxMDM5NzY4NTA2LDg3MD
MyOTE4MiwxMDM2ODcxMjc4LDIwNDgzMjQ2NjZdfQ==
-->