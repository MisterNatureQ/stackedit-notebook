# [[Golang] protoactor-go 101: Introduction to golang's actor model implementation](https://blog.oklahome.net/2018/07/protoactor-go-introduction.html)

A year has passed since I officially launched  [go-sarah](https://github.com/oklahomer/go-sarah). While this bot framework had been a great help with my ChatOps, I found myself becoming more and more interested in designing a chat system as a whole. Not just a text-based communication tool or its varied extension; but as a customizable event aggregation system that provides and consumes any conceivable event varied from virtual to real-life. In the course of its server-side design, Golang’s actor model implementation,  [protoactor-go](https://github.com/AsynkronIT/protoactor-go/), seemed like a good option. However, protoactor-go is still in its Beta phase and has less documentation at this point in time. This article describes what I have learned about this product. The basic of actor model is not going to be covered, but for those who are interested, my previous post “[Yet another Akka introduction for dummies](https://blog.oklahome.net/2016/01/akka-introduction-for-dummies.html)“ might be a help.

Unless otherwise noted, this introduction is based on the latest version as of  [2018-07-21](https://github.com/AsynkronIT/protoactor-go/commit/3992780c0af683deb5ec3746f4ec5845139c6e42).

-   [Terms, Concepts, and Common Types](https://blog.oklahome.net/2018/07/protoactor-go-introduction.html#toc_0)
    -   [Message](https://blog.oklahome.net/2018/07/protoactor-go-introduction.html#toc_1)
    -   [PID](https://blog.oklahome.net/2018/07/protoactor-go-introduction.html#toc_2)
    -   [Process](https://blog.oklahome.net/2018/07/protoactor-go-introduction.html#toc_3)
        -   [Router](https://blog.oklahome.net/2018/07/protoactor-go-introduction.html#toc_4)
        -   [Local process](https://blog.oklahome.net/2018/07/protoactor-go-introduction.html#toc_5)
        -   [Remote process](https://blog.oklahome.net/2018/07/protoactor-go-introduction.html#toc_6)
        -   [Guardian process](https://blog.oklahome.net/2018/07/protoactor-go-introduction.html#toc_7)
        -   [Future process](https://blog.oklahome.net/2018/07/protoactor-go-introduction.html#toc_8)
        -   [Dead letter process](https://blog.oklahome.net/2018/07/protoactor-go-introduction.html#toc_9)
    -   [Mailbox](https://blog.oklahome.net/2018/07/protoactor-go-introduction.html#toc_10)
    -   [Context](https://blog.oklahome.net/2018/07/protoactor-go-introduction.html#toc_11)
    -   [Middleware](https://blog.oklahome.net/2018/07/protoactor-go-introduction.html#toc_12)
    -   [Router](https://blog.oklahome.net/2018/07/protoactor-go-introduction.html#toc_13)
    -   [Event Stream](https://blog.oklahome.net/2018/07/protoactor-go-introduction.html#toc_14)
-   [Actor Construction](https://blog.oklahome.net/2018/07/protoactor-go-introduction.html#toc_15)
-   [Spawner – Construct actor and initiate its lifecycle](https://blog.oklahome.net/2018/07/protoactor-go-introduction.html#toc_16)
-   [Child Actor construction](https://blog.oklahome.net/2018/07/protoactor-go-introduction.html#toc_17)
-   [Supervisor Strategy](https://blog.oklahome.net/2018/07/protoactor-go-introduction.html#toc_18)
-   [Upcoming Interface Change](https://blog.oklahome.net/2018/07/protoactor-go-introduction.html#toc_19)
-   [Further Readings](https://blog.oklahome.net/2018/07/protoactor-go-introduction.html#toc_20)

# Terms, Concepts, and Common Types

## Message

With the nature of the actor model, a message plays an important part to let actors interact with others. Messages internally fall into two categories:

-   User message … Messages defined by developers for actor interaction.  
    
-   System message … Messages defined by protoactor-go for internal use that mainly handles the actor lifecycle.  
    

## PID

[actor.PID](https://github.com/AsynkronIT/protoactor-go/blob/3992780c0af683deb5ec3746f4ec5845139c6e42/actor/pid.go#L12)  is a container that combines a unique identifier, the address and a reference to actor.Process altogether. Since this provides interfaces for others to interact with the underlying actor, this can be seen as an  [actor reference](https://doc.akka.io/docs/akka/2.5/general/addressing.html#what-is-an-actor-reference-)  if one is familiar with Akka. Or simply a Pid if familiar with Erlang. However, this is very important to remember that an actor process is not the only entity that a PID encapsulates.

## Process

[actor.Process](https://github.com/AsynkronIT/protoactor-go/blob/3992780c0af683deb5ec3746f4ec5845139c6e42/actor/process.go) defines a common interface that all interacting “process” must implement. In this project, the concepts of process and PID are quite similar to those of Erlang. Understanding that PID is not necessarily a representation of an actor process is vital when referring to actor messaging context. This distinction and its importance are described in the follow-up article, [[Golang] protoactor-go 101: How actors communicate with each other.](https://blog.oklahome.net/2018/09/protoactor-go-messaging-protocol.html) Its implementation varies depending on each role as below:

### Router

[router.process](https://github.com/AsynkronIT/protoactor-go/blob/3992780c0af683deb5ec3746f4ec5845139c6e42/router/process.go#L12)  receives a message and broadcasts it to all subordinating actors: “routees.”

### Local process

[actor.localProcess](https://github.com/AsynkronIT/protoactor-go/blob/3992780c0af683deb5ec3746f4ec5845139c6e42/actor/local_process.go#L9)  has a reference to a mailbox. On message reception, this enqueues the message to its mailbox so the actor can receive this for further procedure.

### Remote process

On contrary to a local process, this represents an actor that exists in a remote environment. On message reception, this serializes the message and sends it to the destination host.

### Guardian process

When a developer passes a “guardian”’s supervisor strategy for actor constructor, a parent actor is created with this supervisor strategy along with the actor itself. This parent “guardian” actor will take care of the child actor’s uncontrollable state. This should be effective when the constructing actor is the “root actor” – an actor without a parent actor – but customized supervision is still required. When multiple actor constructions contain the same settings for guardian supervision,  [only one guardian actor is created](https://github.com/AsynkronIT/protoactor-go/blob/3992780c0af683deb5ec3746f4ec5845139c6e42/actor/guardian.go#L16)  and this becomes the parent of all actors with the same settings.

### Future process

[actor.futureProcess](https://github.com/AsynkronIT/protoactor-go/blob/3992780c0af683deb5ec3746f4ec5845139c6e42/actor/future.go#L110)  provides some dedicated features for Future related tasks.

### Dead letter process

[actor.deadLetterProcess](https://github.com/AsynkronIT/protoactor-go/blob/3992780c0af683deb5ec3746f4ec5845139c6e42/actor/deadletter.go#L8)  provides features to handle “dead letters.” A dead letter is a message that failed to reach target because, for example, the target actor did not exist or was already stopped. This dead letter process publishes  [actor.DeadLetterEvent](https://github.com/AsynkronIT/protoactor-go/blob/3992780c0af683deb5ec3746f4ec5845139c6e42/actor/deadletter.go#L36)  to the event stream, so a developer can detect the dead letter by subscribing to the event via  [eventstream.Subscribe()](https://github.com/AsynkronIT/protoactor-go/blob/3992780c0af683deb5ec3746f4ec5845139c6e42/eventstream/eventstream.go#L12).

## Mailbox

This works as a queue to receive incoming messages, store them temporarily and pass them to its coupled actor when the actor is ready for message execution. The actor is to receive the message one at a time, execute its task and alter its state if necessary. Mailbox implements  [mailbox.Inbound](https://github.com/AsynkronIT/protoactor-go/blob/3992780c0af683deb5ec3746f4ec5845139c6e42/mailbox/mailbox.go#L26)interface.

-   Default mailbox …  [mailbox.defaultMailbox](https://github.com/AsynkronIT/protoactor-go/blob/3992780c0af683deb5ec3746f4ec5845139c6e42/mailbox/mailbox.go#L40)  not only receives incoming messages as a mailbox.Inbound implementation, but also coordinates the actor invocation schedule with its  [mailbox.Dispatcher](https://github.com/AsynkronIT/protoactor-go/blob/3992780c0af683deb5ec3746f4ec5845139c6e42/mailbox/dispatcher.go#L3)  implementation entity. This mailbox also contains  [mailbox.MessageInvoker](https://github.com/AsynkronIT/protoactor-go/blob/3992780c0af683deb5ec3746f4ec5845139c6e42/mailbox/mailbox.go#L19)implementation as its entity and its methods are called by mailbox.Dispatcher for actor invocation purpose.  [actor.localContext](https://github.com/AsynkronIT/protoactor-go/blob/3992780c0af683deb5ec3746f4ec5845139c6e42/actor/local_context.go#L21)  implements mailbox.MessageInvoker.  
    

## Context

This is equivalent to  [Akka’s ActorCoontext](https://doc.akka.io/api/akka/2.5/akka/actor/ActorContext.html). This contains contextual information and contextual methods for the underlying actor such as below:

-   References to watching actors and methods to watch/unwatch other actors  
    
-   A reference to the actor who sent the currently processing message and a method to access to this  
    
-   Methods to pass a message to another actor  
    
-   etc…  
    

## Middleware

Zero or more pre-registered procedures can be executed around actor invocation, which enables an AOP-like approach to modify behavior.

-   Inbound middleware …  [actor.InboundMiddleware](https://github.com/AsynkronIT/protoactor-go/blob/3992780c0af683deb5ec3746f4ec5845139c6e42/actor/props.go#L5)  is a middleware that is executed on message reception. A developer may register one or more middleware via Props.WithMiddleware().  
    
-   Outbound middleware …  [actor.OutboundMiddleware](https://github.com/AsynkronIT/protoactor-go/blob/3992780c0af683deb5ec3746f4ec5845139c6e42/actor/props.go#L6)  is a middleware that is executed on message sending. A developer may register one or more middleware via Props.WithOutboundMiddleware().  
    

## Router

A sub-package,  `router`, provides a series of mechanism that routes a given message to one or more of its routees.

-   [Broadcast router](https://github.com/AsynkronIT/protoactor-go/blob/3992780c0af683deb5ec3746f4ec5845139c6e42/router/broadcast_router.go)  … Broadcast given message to all of its routee actors.  
    
-   [Round robin router](https://github.com/AsynkronIT/protoactor-go/blob/3992780c0af683deb5ec3746f4ec5845139c6e42/router/roundrobin_router.go)  … Send given message to one of its routee actors chosen by round-robin manner  
    
-   [Random router](https://github.com/AsynkronIT/protoactor-go/blob/3992780c0af683deb5ec3746f4ec5845139c6e42/router/random_router.go)  … Send given message to a randomly chosen routee actor.  
    

## Event Stream

[eventstream.EventStream](https://github.com/AsynkronIT/protoactor-go/blob/3992780c0af683deb5ec3746f4ec5845139c6e42/eventstream/eventstream.go#L24)  is a mechanism to publish and subscribe given event where the event is an empty interface, interface{}. So the developer can technically publish and subscribe to any desired event. By default an instance of eventstream.EventStream is cached in package local manner and is used to publish and subscribe events such as dead letter messages.

# Actor Construction

To construct a new actor and acquire a reference to this, a developer can feed an actor.Props to  [actor.Spawn](https://github.com/AsynkronIT/protoactor-go/blob/3992780c0af683deb5ec3746f4ec5845139c6e42/actor/spawn.go#L16)  or  [actor.SpawnNamed](https://github.com/AsynkronIT/protoactor-go/blob/3992780c0af683deb5ec3746f4ec5845139c6e42/actor/spawn.go#L29). The struct called  [actor.Props](https://github.com/AsynkronIT/protoactor-go/blob/3992780c0af683deb5ec3746f4ec5845139c6e42/actor/props.go#L9)  is a set of configuration for actor construction. actor.Props can be initialized with helper functions listed below:

-   [actor.FromProducer()](https://github.com/AsynkronIT/protoactor-go/blob/3992780c0af683deb5ec3746f4ec5845139c6e42/actor/actor.go#L24)  … Pass a function that returns an actor.Actor implementation. This returns a pointer to actor.Props, which contains a set of configurations for actor construction.  
    
-   [actor.FromFunc()](https://github.com/AsynkronIT/protoactor-go/blob/3992780c0af683deb5ec3746f4ec5845139c6e42/actor/actor.go#L29)  … Pass a function that satisfies actor.ActorFunc type, which receives exactly the same arguments as Actor.Recieve(). This is a handy wrapper of actor.FromProducer.  
    
-   [actor.FromSpawnFunc()](https://github.com/AsynkronIT/protoactor-go/blob/3992780c0af683deb5ec3746f4ec5845139c6e42/actor/actor.go#L33)  … Pass a function that satisfies actor.SpawnFunc type. on actor construction, this function is called with a series of arguments containing id, actor.Props and parent PID to construct a new actor. When this function is not set, actor.DefaultSpawner is used.  
    
-   actor.FromInstance() … Deprecated.  
    

Additional configuration can be added via its setter methods with “With” prefix.  [See example code](https://github.com/AsynkronIT/protoactor-go/tree/3992780c0af683deb5ec3746f4ec5845139c6e42/examples/helloworld).

# Spawner – Construct actor and initiate its lifecycle

A developer feeds a prepared actor.Props to actor.Spawn() or actor.SpawnNamed() depending on the requirement to initialize an actor, its context, and its mailbox. In any construction flow,  [Props.spawn()](https://github.com/AsynkronIT/protoactor-go/blob/3992780c0af683deb5ec3746f4ec5845139c6e42/actor/props.go#L41)is called. To alter this spawning behavior, an alternative function can be set with actor.FromSpawnFunc() or Props.WithSpawnFunc() to override the default behavior. When none is set,  [actor.DefaultSpawner](https://github.com/AsynkronIT/protoactor-go/blob/3992780c0af683deb5ec3746f4ec5845139c6e42/actor/spawn.go#L13)  is used by default. Its behavior is as below:

-   The default spawner creates an instance of  [actor.localProcess](https://github.com/AsynkronIT/protoactor-go/blob/3992780c0af683deb5ec3746f4ec5845139c6e42/actor/local_process.go#L9), which is an actor.Process implementation.  
    
-   Add the instance to  [actor.ProcessRegistry](https://github.com/AsynkronIT/protoactor-go/blob/3992780c0af683deb5ec3746f4ec5845139c6e42/actor/process_registry.go#L62).  
    -   The registry returns an error if given id is already registered.  
        
-   Create new  [actor.localContext](https://github.com/AsynkronIT/protoactor-go/blob/3992780c0af683deb5ec3746f4ec5845139c6e42/actor/local_context.go#L21)  which is an actor.Context implementation. This stores all contextual data.  
    
-   Mailbox is created for the context. To modify the behavior of mailbox, use  [Props.WithDispatcher()](https://github.com/AsynkronIT/protoactor-go/blob/3992780c0af683deb5ec3746f4ec5845139c6e42/actor/props.go#L78)  and  [Props.WithMailbox()](https://github.com/AsynkronIT/protoactor-go/blob/3992780c0af683deb5ec3746f4ec5845139c6e42/actor/props.go#L60).  
    
-   Created mailbox is stored in the actor.localProcess instance.  
    
-   The pointer to the process is set to actor.PID’s field.  
    
-   actor.localContext also has a reference to the actor.PID as “self.”  
    
-   Start mailbox  
    
-   Enqueue mailbox a startedMessage as a system message which is an instance of actor.Started.  
    

When construction is done and the actor lifecycle is successfully started, actor.PID for the new actor is returned.

# Child Actor construction

With the introduced actor construction procedure, a developer can create any “root actor,” an actor with no parent. To achieve a hierarchized actor system, use actor.Context’s Spawn() or SpawnNamed() method. Those methods work similarly to actor.Spawn() and actor.SpawnNamed(), but the single and biggest difference is that they create a parent-child relationship between the spawning actor and the newly created actor. They work as below:

1.  Check if Props.guardianStrategy is set  
    -   If set, it panics. Because the calling actor is going to be the parent and be obligated to be a supervisor, there is no need to set one. This strategy is to create a parent actor for customized supervision as introduced in the first section.  
        
2.  Call Props.spawn()  
    -   The ID has a form of {parent-id}/{child-id}  
        
    -   Own PID is set as a parent for the new actor  
        
3.  Add created actors actor.PID to its children  
    
4.  Start watching the created actor.PID to subscribe its lifecycle event  
    

[See example code.](https://github.com/AsynkronIT/protoactor-go/tree/3992780c0af683deb5ec3746f4ec5845139c6e42/examples/supervision)

# Supervisor Strategy

This is a parent actor’s responsibility to take care of its child actor’s exceptional state. When a child actor can no longer control its state, based on the “let-it-crash” philosophy, child actor notifies such situation to parent actor by panic(). The parent actor receives such notification with recover() and decides how to treat such failing actor. This decision is made by a customizable  [actor.SupervisorStrategy](https://github.com/AsynkronIT/protoactor-go/blob/3992780c0af683deb5ec3746f4ec5845139c6e42/actor/supervision.go#L9). When no strategy is explicitly set by a developer,  [actor.defaultSupervisorStrategy](https://github.com/AsynkronIT/protoactor-go/blob/3992780c0af683deb5ec3746f4ec5845139c6e42/actor/supervision.go#L36)  is set on actor construction.

The supervision flow is as follows:

1.  A mailbox passes a message to Actor.Recieve() via target actor context’s localContext.InvokeUserMessage().  
    
2.  In Actor.Receive(), the actor calls panic().  
    
3.  Caller mailbox catches such uncontrollable state with recover().  
    
4.  The mailbox calls localContext.EscalateFailure(), where localContext is that of the failing actor.  
    1.  In localContext.EscalateFailure(), this tells itself to suspend any incoming message till recovery is done.  
        
    2.  Create actor.Failure instance that holds failing reason and other statistical information, where “reason” is the argument passed to panic().  
        
    3.  Judges if the failing actor has any parent  
        -   If none is found, the failing actor is the “root actor” so the actor.Failure is passed to actor.handleRootFactor().  
            
        -   If found, this passes actor.Failure to parent’s PID.sendSystemMessage() to notify failing state  
            1.  The message is enqueued to parent actor’s mailbox  
                
            2.  Parent’s mailbox calls its localContext.InvokeSystemMessage.  
                
            3.  actor.Failure is passed to localContext.handleFailure  
                
            4.  If its actor.Actor entity itself implements actor.SupervisorStrategy, its HandleFailure() is called.  
                
            5.  If not, its supervisor entity’s handleFailure() is called.  
                
            6.  In HandleFailure(), decide recovery policy and call localContext.(ResumeChildren|RestartChildren|StopChildren|EscalateFailure).  
                

[See example code.](https://github.com/AsynkronIT/protoactor-go/tree/3992780c0af683deb5ec3746f4ec5845139c6e42/examples/supervision)

# Upcoming Interface Change

A huge interface change is expected according to the issue “[Design / API Changes upcoming](https://github.com/AsynkronIT/protoactor-go/issues/239).”

# Further Readings

See below articles for more information:

-   [protoactor-go 101: How actors communicate with each other](https://blog.oklahome.net/2018/09/protoactor-go-messaging-protocol.html)  
    
-   [protoactor-go 101: How actor.Future works to synchronize concurrent task execution](https://blog.oklahome.net/2018/11/protoactor-go-how-future-works.html)  
    
-   [protoactor-go 201: How middleware works to intercept incoming and outgoing messages](https://blog.oklahome.net/2018/11/protoactor-go-middleware.html)  
    
-   [protoactor-go 201: Use plugins to add behaviors to an actor](https://blog.oklahome.net/2018/12/protoactor-go-use-plugin-to-add-behavior.html)








# [Golang] protoactor-go 101：golang的演员模型实现简介

[自我](https://translate.googleusercontent.com/translate_c?depth=1&hl=zh-CN&prev=search&rurl=translate.google.com&sl=en&sp=nmt4&u=https://github.com/oklahomer/go-sarah&xid=17259,15700023,15700124,15700186,15700191,15700201,15700237,15700242,15700248&usg=ALkJrhhuOQksBPd3Q9-EqNP1_tcNN-sv2A)正式推出[go-sarah](https://translate.googleusercontent.com/translate_c?depth=1&hl=zh-CN&prev=search&rurl=translate.google.com&sl=en&sp=nmt4&u=https://github.com/oklahomer/go-sarah&xid=17259,15700023,15700124,15700186,15700191,15700201,15700237,15700242,15700248&usg=ALkJrhhuOQksBPd3Q9-EqNP1_tcNN-sv2A)以来已过去一年。  虽然这个机器人框架对我的ChatOps有很大帮助，但我发现自己对整个聊天系统的设计越来越感兴趣。  不仅仅是基于文本的通信工具或其不同的扩展;  但作为一个可定制的事件聚合系统，提供和消费从虚拟到现实的各种可能的事件。  在服务器端设计过程中，Golang的actor模型实现[protoactor-go](https://translate.googleusercontent.com/translate_c?depth=1&hl=zh-CN&prev=search&rurl=translate.google.com&sl=en&sp=nmt4&u=https://github.com/AsynkronIT/protoactor-go/&xid=17259,15700023,15700124,15700186,15700191,15700201,15700237,15700242,15700248&usg=ALkJrhgO73EYj--73UOFhIjMp6vZ8VNsjQ)似乎是个不错的选择。  但是，protoactor-go仍处于测试阶段，此时文档记录较少。  本文介绍了我对该产品的了解。  演员模型的基础不会被覆盖，但对于那些感兴趣的人，我之前的帖子“  [另一个Akka介绍傻瓜](https://translate.googleusercontent.com/translate_c?depth=1&hl=zh-CN&prev=search&rurl=translate.google.com&sl=en&sp=nmt4&u=https://blog.oklahome.net/2016/01/akka-introduction-for-dummies.html&xid=17259,15700023,15700124,15700186,15700191,15700201,15700237,15700242,15700248&usg=ALkJrhhVP-L6YfWYr_2veWMdF16YUy-4oA)  ”可能是一个帮助。

除非另有说明，否则此介绍基于[2018-07-21](https://translate.googleusercontent.com/translate_c?depth=1&hl=zh-CN&prev=search&rurl=translate.google.com&sl=en&sp=nmt4&u=https://github.com/AsynkronIT/protoactor-go/commit/3992780c0af683deb5ec3746f4ec5845139c6e42&xid=17259,15700023,15700124,15700186,15700191,15700201,15700237,15700242,15700248&usg=ALkJrhjegSobVdebXXt80KI6taAMHIweeA)的最新版本。

-   [术语，概念和常见类型](https://translate.googleusercontent.com/translate_c?depth=1&hl=zh-CN&prev=search&rurl=translate.google.com&sl=en&sp=nmt4&u=https://blog.oklahome.net/2018/07/protoactor-go-introduction.html&xid=17259,15700023,15700124,15700186,15700191,15700201,15700237,15700242,15700248&usg=ALkJrhh3LnHnZU_HYyH9M659eEmy_10RFw#toc_0)
    -   [信息](https://translate.googleusercontent.com/translate_c?depth=1&hl=zh-CN&prev=search&rurl=translate.google.com&sl=en&sp=nmt4&u=https://blog.oklahome.net/2018/07/protoactor-go-introduction.html&xid=17259,15700023,15700124,15700186,15700191,15700201,15700237,15700242,15700248&usg=ALkJrhh3LnHnZU_HYyH9M659eEmy_10RFw#toc_1)
    -   [PID](https://translate.googleusercontent.com/translate_c?depth=1&hl=zh-CN&prev=search&rurl=translate.google.com&sl=en&sp=nmt4&u=https://blog.oklahome.net/2018/07/protoactor-go-introduction.html&xid=17259,15700023,15700124,15700186,15700191,15700201,15700237,15700242,15700248&usg=ALkJrhh3LnHnZU_HYyH9M659eEmy_10RFw#toc_2)
    -   [处理](https://translate.googleusercontent.com/translate_c?depth=1&hl=zh-CN&prev=search&rurl=translate.google.com&sl=en&sp=nmt4&u=https://blog.oklahome.net/2018/07/protoactor-go-introduction.html&xid=17259,15700023,15700124,15700186,15700191,15700201,15700237,15700242,15700248&usg=ALkJrhh3LnHnZU_HYyH9M659eEmy_10RFw#toc_3)
        -   [路由器](https://translate.googleusercontent.com/translate_c?depth=1&hl=zh-CN&prev=search&rurl=translate.google.com&sl=en&sp=nmt4&u=https://blog.oklahome.net/2018/07/protoactor-go-introduction.html&xid=17259,15700023,15700124,15700186,15700191,15700201,15700237,15700242,15700248&usg=ALkJrhh3LnHnZU_HYyH9M659eEmy_10RFw#toc_4)
        -   [本地流程](https://translate.googleusercontent.com/translate_c?depth=1&hl=zh-CN&prev=search&rurl=translate.google.com&sl=en&sp=nmt4&u=https://blog.oklahome.net/2018/07/protoactor-go-introduction.html&xid=17259,15700023,15700124,15700186,15700191,15700201,15700237,15700242,15700248&usg=ALkJrhh3LnHnZU_HYyH9M659eEmy_10RFw#toc_5)
        -   [远程过程](https://translate.googleusercontent.com/translate_c?depth=1&hl=zh-CN&prev=search&rurl=translate.google.com&sl=en&sp=nmt4&u=https://blog.oklahome.net/2018/07/protoactor-go-introduction.html&xid=17259,15700023,15700124,15700186,15700191,15700201,15700237,15700242,15700248&usg=ALkJrhh3LnHnZU_HYyH9M659eEmy_10RFw#toc_6)
        -   [守护进程](https://translate.googleusercontent.com/translate_c?depth=1&hl=zh-CN&prev=search&rurl=translate.google.com&sl=en&sp=nmt4&u=https://blog.oklahome.net/2018/07/protoactor-go-introduction.html&xid=17259,15700023,15700124,15700186,15700191,15700201,15700237,15700242,15700248&usg=ALkJrhh3LnHnZU_HYyH9M659eEmy_10RFw#toc_7)
        -   [未来的过程](https://translate.googleusercontent.com/translate_c?depth=1&hl=zh-CN&prev=search&rurl=translate.google.com&sl=en&sp=nmt4&u=https://blog.oklahome.net/2018/07/protoactor-go-introduction.html&xid=17259,15700023,15700124,15700186,15700191,15700201,15700237,15700242,15700248&usg=ALkJrhh3LnHnZU_HYyH9M659eEmy_10RFw#toc_8)
        -   [死信过程](https://translate.googleusercontent.com/translate_c?depth=1&hl=zh-CN&prev=search&rurl=translate.google.com&sl=en&sp=nmt4&u=https://blog.oklahome.net/2018/07/protoactor-go-introduction.html&xid=17259,15700023,15700124,15700186,15700191,15700201,15700237,15700242,15700248&usg=ALkJrhh3LnHnZU_HYyH9M659eEmy_10RFw#toc_9)
    -   [邮箱](https://translate.googleusercontent.com/translate_c?depth=1&hl=zh-CN&prev=search&rurl=translate.google.com&sl=en&sp=nmt4&u=https://blog.oklahome.net/2018/07/protoactor-go-introduction.html&xid=17259,15700023,15700124,15700186,15700191,15700201,15700237,15700242,15700248&usg=ALkJrhh3LnHnZU_HYyH9M659eEmy_10RFw#toc_10)
    -   [上下文](https://translate.googleusercontent.com/translate_c?depth=1&hl=zh-CN&prev=search&rurl=translate.google.com&sl=en&sp=nmt4&u=https://blog.oklahome.net/2018/07/protoactor-go-introduction.html&xid=17259,15700023,15700124,15700186,15700191,15700201,15700237,15700242,15700248&usg=ALkJrhh3LnHnZU_HYyH9M659eEmy_10RFw#toc_11)
    -   [中间件](https://translate.googleusercontent.com/translate_c?depth=1&hl=zh-CN&prev=search&rurl=translate.google.com&sl=en&sp=nmt4&u=https://blog.oklahome.net/2018/07/protoactor-go-introduction.html&xid=17259,15700023,15700124,15700186,15700191,15700201,15700237,15700242,15700248&usg=ALkJrhh3LnHnZU_HYyH9M659eEmy_10RFw#toc_12)
    -   [路由器](https://translate.googleusercontent.com/translate_c?depth=1&hl=zh-CN&prev=search&rurl=translate.google.com&sl=en&sp=nmt4&u=https://blog.oklahome.net/2018/07/protoactor-go-introduction.html&xid=17259,15700023,15700124,15700186,15700191,15700201,15700237,15700242,15700248&usg=ALkJrhh3LnHnZU_HYyH9M659eEmy_10RFw#toc_13)
    -   [事件流](https://translate.googleusercontent.com/translate_c?depth=1&hl=zh-CN&prev=search&rurl=translate.google.com&sl=en&sp=nmt4&u=https://blog.oklahome.net/2018/07/protoactor-go-introduction.html&xid=17259,15700023,15700124,15700186,15700191,15700201,15700237,15700242,15700248&usg=ALkJrhh3LnHnZU_HYyH9M659eEmy_10RFw#toc_14)
-   [演员建设](https://translate.googleusercontent.com/translate_c?depth=1&hl=zh-CN&prev=search&rurl=translate.google.com&sl=en&sp=nmt4&u=https://blog.oklahome.net/2018/07/protoactor-go-introduction.html&xid=17259,15700023,15700124,15700186,15700191,15700201,15700237,15700242,15700248&usg=ALkJrhh3LnHnZU_HYyH9M659eEmy_10RFw#toc_15)
-   [Spawner - 构建actor并启动其生命周期](https://translate.googleusercontent.com/translate_c?depth=1&hl=zh-CN&prev=search&rurl=translate.google.com&sl=en&sp=nmt4&u=https://blog.oklahome.net/2018/07/protoactor-go-introduction.html&xid=17259,15700023,15700124,15700186,15700191,15700201,15700237,15700242,15700248&usg=ALkJrhh3LnHnZU_HYyH9M659eEmy_10RFw#toc_16)
-   [儿童演员建设](https://translate.googleusercontent.com/translate_c?depth=1&hl=zh-CN&prev=search&rurl=translate.google.com&sl=en&sp=nmt4&u=https://blog.oklahome.net/2018/07/protoactor-go-introduction.html&xid=17259,15700023,15700124,15700186,15700191,15700201,15700237,15700242,15700248&usg=ALkJrhh3LnHnZU_HYyH9M659eEmy_10RFw#toc_17)
-   [主管战略](https://translate.googleusercontent.com/translate_c?depth=1&hl=zh-CN&prev=search&rurl=translate.google.com&sl=en&sp=nmt4&u=https://blog.oklahome.net/2018/07/protoactor-go-introduction.html&xid=17259,15700023,15700124,15700186,15700191,15700201,15700237,15700242,15700248&usg=ALkJrhh3LnHnZU_HYyH9M659eEmy_10RFw#toc_18)
-   [即将到来的界面变化](https://translate.googleusercontent.com/translate_c?depth=1&hl=zh-CN&prev=search&rurl=translate.google.com&sl=en&sp=nmt4&u=https://blog.oklahome.net/2018/07/protoactor-go-introduction.html&xid=17259,15700023,15700124,15700186,15700191,15700201,15700237,15700242,15700248&usg=ALkJrhh3LnHnZU_HYyH9M659eEmy_10RFw#toc_19)
-   [进一步阅读](https://translate.googleusercontent.com/translate_c?depth=1&hl=zh-CN&prev=search&rurl=translate.google.com&sl=en&sp=nmt4&u=https://blog.oklahome.net/2018/07/protoactor-go-introduction.html&xid=17259,15700023,15700124,15700186,15700191,15700201,15700237,15700242,15700248&usg=ALkJrhh3LnHnZU_HYyH9M659eEmy_10RFw#toc_20)

# 术语，概念和常见类型

## 信息

根据演员模型的本质，消息对于让演员与他人互动起着重要作用。  消息内部分为两类：

-   用户消息...开发人员为演员交互定义的消息。  
    
-   系统消息...由protoactor定义的消息 - 供内部使用，主要处理actor的生命周期。  
    

## PID

[actor.PID](https://github.com/AsynkronIT/protoactor-go/blob/3992780c0af683deb5ec3746f4ec5845139c6e42/actor/pid.go#L12)是一个容器，它结合了唯一标识符，地址和对actor.Process的引用。  由于这为其他人提供了与底层actor交互的接口，如果熟悉Akka，这可以被视为一个[actor参考](https://translate.googleusercontent.com/translate_c?depth=1&hl=zh-CN&prev=search&rurl=translate.google.com&sl=en&sp=nmt4&u=https://doc.akka.io/docs/akka/2.5/general/addressing.html&xid=17259,15700023,15700124,15700186,15700191,15700201,15700237,15700242,15700248&usg=ALkJrhgdSgfSICjIHDfvoxVJ-1me8d7n6A#what-is-an-actor-reference-)  。  或者只是熟悉Erlang的Pid。  但是，要记住actor进程不是PID封装的唯一实体，这一点非常重要。

## 处理

[actor.Process](https://github.com/AsynkronIT/protoactor-go/blob/3992780c0af683deb5ec3746f4ec5845139c6e42/actor/process.go)定义了一个所有交互“进程”必须实现的公共接口。  在这个项目中，进程和PID的概念与Erlang的概念非常相似。  在引用actor消息传递上下文时，理解PID不一定是actor过程的表示是至关重要的。  这一区别及其重要性在后续文章[[Golang] protoactor-go 101：演员之间如何相互沟通中](https://translate.googleusercontent.com/translate_c?depth=1&hl=zh-CN&prev=search&rurl=translate.google.com&sl=en&sp=nmt4&u=https://blog.oklahome.net/2018/09/protoactor-go-messaging-protocol.html&xid=17259,15700023,15700124,15700186,15700191,15700201,15700237,15700242,15700248&usg=ALkJrhi8UkvQn3XZbbVsJ0UES-xFZBwRQQ)有所描述[。](https://translate.googleusercontent.com/translate_c?depth=1&hl=zh-CN&prev=search&rurl=translate.google.com&sl=en&sp=nmt4&u=https://blog.oklahome.net/2018/09/protoactor-go-messaging-protocol.html&xid=17259,15700023,15700124,15700186,15700191,15700201,15700237,15700242,15700248&usg=ALkJrhi8UkvQn3XZbbVsJ0UES-xFZBwRQQ)  它的实施取决于每个角色，如下所示：

### 路由器

[router.process](https://github.com/AsynkronIT/protoactor-go/blob/3992780c0af683deb5ec3746f4ec5845139c6e42/router/process.go#L12)接收消息并将其广播给所有从属actor：“routees”。

### 本地流程

[actor.localProcess](https://github.com/AsynkronIT/protoactor-go/blob/3992780c0af683deb5ec3746f4ec5845139c6e42/actor/local_process.go#L9)具有对邮箱的引用。  在消息接收时，这会将消息排入其邮箱，以便actor可以接收此消息以进行进一步的操作。

### 远程过程

与本地进程相反，它表示存在于远程环境中的actor。  在消息接收时，此序列化消息并将其发送到目标主机。

### 守护进程

当开发人员为演员构造函数传递“监护人”的主管策略时，将使用此监督策略以及演员本身创建父演员。  这位父母“守护者”演员将照顾儿童演员的无法控制的状态。  当构造演员是“根演员” - 没有父演员的演员 - 时，这应该是有效的 - 但仍然需要定制监督。  当多个actor构造包含相同的监护人监督设置时，  [只创建一个监护人演员](https://github.com/AsynkronIT/protoactor-go/blob/3992780c0af683deb5ec3746f4ec5845139c6e42/actor/guardian.go#L16)  ，这将成为具有相同设置的所有演员的父级。

### 未来的过程

[actor.futureProcess](https://github.com/AsynkronIT/protoactor-go/blob/3992780c0af683deb5ec3746f4ec5845139c6e42/actor/future.go#L110)为Future相关任务提供了一些专用功能。

### 死信过程

[actor.deadLetterProcess](https://github.com/AsynkronIT/protoactor-go/blob/3992780c0af683deb5ec3746f4ec5845139c6e42/actor/deadletter.go#L8)提供处理“死信”的功能。死信是无法达到目标的消息，例如，目标actor不存在或已经停止。  此死信进程将[actor.DeadLetterEvent](https://github.com/AsynkronIT/protoactor-go/blob/3992780c0af683deb5ec3746f4ec5845139c6e42/actor/deadletter.go#L36)发布到事件流，因此开发人员可以通过[eventstream.Subscribe（）](https://github.com/AsynkronIT/protoactor-go/blob/3992780c0af683deb5ec3746f4ec5845139c6e42/eventstream/eventstream.go#L12)订阅事件来检测[死信](https://github.com/AsynkronIT/protoactor-go/blob/3992780c0af683deb5ec3746f4ec5845139c6e42/eventstream/eventstream.go#L12)  。

## 邮箱

这用作接收传入消息的队列，临时存储它们并在actor准备好执行消息时将它们传递给它的耦合actor。  演员将一次接收一条消息，执行其任务并在必要时更改其状态。  邮箱实现[mailbox.Inbound](https://github.com/AsynkronIT/protoactor-go/blob/3992780c0af683deb5ec3746f4ec5845139c6e42/mailbox/mailbox.go#L26)接口。

-   默认邮箱...  [mailbox.defaultMailbox](https://github.com/AsynkronIT/protoactor-go/blob/3992780c0af683deb5ec3746f4ec5845139c6e42/mailbox/mailbox.go#L40)不仅接收传入消息作为mailbox.Inbound实现，还协调actor调用计划及其[mailbox.Dispatcher](https://github.com/AsynkronIT/protoactor-go/blob/3992780c0af683deb5ec3746f4ec5845139c6e42/mailbox/dispatcher.go#L3)实现实体。  此邮箱还包含[mailbox.MessageInvoker](https://github.com/AsynkronIT/protoactor-go/blob/3992780c0af683deb5ec3746f4ec5845139c6e42/mailbox/mailbox.go#L19)实现作为其实体，其方法由mailbox.Dispatcher调用以进行actor调用。  [actor.localContext](https://github.com/AsynkronIT/protoactor-go/blob/3992780c0af683deb5ec3746f4ec5845139c6e42/actor/local_context.go#L21)实现了mailbox.MessageInvoker。  
    

## 上下文

这相当于[Akka的ActorCoontext](https://translate.googleusercontent.com/translate_c?depth=1&hl=zh-CN&prev=search&rurl=translate.google.com&sl=en&sp=nmt4&u=https://doc.akka.io/api/akka/2.5/akka/actor/ActorContext.html&xid=17259,15700023,15700124,15700186,15700191,15700201,15700237,15700242,15700248&usg=ALkJrhiWFMwa_l2I1_65PMcx_59TngQqrQ)  。  这包含底层参与者的上下文信息和上下文方法，如下所示：

-   参考观看演员和观看/观看其他演员的方法  
    
-   对发送当前处理消息的actor的引用以及访问它的方法  
    
-   将消息传递给另一个actor的方法  
    
-   等等…  
    

## 中间件

可以围绕actor调用执行零个或多个预先注册的过程，这使得类似AOP的方法能够修改行为。

-   入站中间件...  [actor.InboundMiddleware](https://github.com/AsynkronIT/protoactor-go/blob/3992780c0af683deb5ec3746f4ec5845139c6e42/actor/props.go#L5)是一个在消息接收时执行的中间件。  开发人员可以通过Props.WithMiddleware（）注册一个或多个中间件。  
    
-   出站中间件...  [actor.OutboundMiddleware](https://github.com/AsynkronIT/protoactor-go/blob/3992780c0af683deb5ec3746f4ec5845139c6e42/actor/props.go#L6)是一个在消息发送时执行的中间件。  开发人员可以通过Props.WithOutb​​oundMiddleware（）注册一个或多个中间件。  
    

## 路由器

子包`router`提供了一系列将给定消息路由到其一个或多个路由的机制。

-   [广播路由器](https://github.com/AsynkronIT/protoactor-go/blob/3992780c0af683deb5ec3746f4ec5845139c6e42/router/broadcast_router.go)  ...向所有路由演员广播给定消息。  
    
-   [循环路由器](https://github.com/AsynkronIT/protoactor-go/blob/3992780c0af683deb5ec3746f4ec5845139c6e42/router/roundrobin_router.go)  ...将给定的消息发送给通过循环方式选择的其中一个特工  
    
-   [随机路由器](https://github.com/AsynkronIT/protoactor-go/blob/3992780c0af683deb5ec3746f4ec5845139c6e42/router/random_router.go)  ...将给定的消息发送给随机选择的路由器角色。  
    

## 事件流

[eventstream.EventStream](https://github.com/AsynkronIT/protoactor-go/blob/3992780c0af683deb5ec3746f4ec5845139c6e42/eventstream/eventstream.go#L24)是一种发布和订阅给定事件的机制，其中事件是一个空接口interface {}。  因此，开发人员可以在技术上发布和订阅任何所需的事件。  默认情况下，eventstream.EventStream的实例以包本地方式缓存，用于发布和订阅死信消息等事件。

# 演员建设

要构造一个新的actor并获得对它的引用，开发人员可以将actor.Props提供给[actor.Spawn](https://github.com/AsynkronIT/protoactor-go/blob/3992780c0af683deb5ec3746f4ec5845139c6e42/actor/spawn.go#L16)或[actor.SpawnNamed](https://github.com/AsynkronIT/protoactor-go/blob/3992780c0af683deb5ec3746f4ec5845139c6e42/actor/spawn.go#L29)  。  名为[actor.Props](https://github.com/AsynkronIT/protoactor-go/blob/3992780c0af683deb5ec3746f4ec5845139c6e42/actor/props.go#L9)的结构是一组用于actor构造的配置。  可以使用下面列出的辅助函数初始化actor.Props：

-   [actor.FromProducer（）](https://github.com/AsynkronIT/protoactor-go/blob/3992780c0af683deb5ec3746f4ec5845139c6e42/actor/actor.go#L24)  ...传递一个返回actor.Actor实现的函数。  这将返回指向actor.Props的指针，该指针包含一组actor构造的配置。  
    
-   [actor.FromFunc（）](https://github.com/AsynkronIT/protoactor-go/blob/3992780c0af683deb5ec3746f4ec5845139c6e42/actor/actor.go#L29)  ...传递一个满足actor.ActorFunc类型的函数，它接收与Actor.Recieve（）完全相同的参数。  这是一个方便的actor.FromProducer包装器。  
    
-   [actor.FromSpawnFunc（）](https://github.com/AsynkronIT/protoactor-go/blob/3992780c0af683deb5ec3746f4ec5845139c6e42/actor/actor.go#L33)  ...传递一个满足actor.SpawnFunc类型的函数。  在actor构造中，使用包含id，actor.Props和父PID的一系列参数调用此函数来构造新的actor。  如果未设置此功能，则使用actor.DefaultSpawner。  
    
-   actor.FromInstance（）...已弃用。  
    

可以通过其setter方法使用“With”前缀添加其他配置。  [请参阅示例代码](https://translate.googleusercontent.com/translate_c?depth=1&hl=zh-CN&prev=search&rurl=translate.google.com&sl=en&sp=nmt4&u=https://github.com/AsynkronIT/protoactor-go/tree/3992780c0af683deb5ec3746f4ec5845139c6e42/examples/helloworld&xid=17259,15700023,15700124,15700186,15700191,15700201,15700237,15700242,15700248&usg=ALkJrhj17vtakxlsQamsi5YmdAecR0kylQ)  。

# Spawner - 构建actor并启动其生命周期

开发人员将准备好的actor.Props提供给actor.Spawn（）或actor.SpawnNamed（），具体取决于初始化actor，其上下文和邮箱的要求。  在任何构造流程中，  [都会](https://github.com/AsynkronIT/protoactor-go/blob/3992780c0af683deb5ec3746f4ec5845139c6e42/actor/props.go#L41)调用[Props.spawn（）](https://github.com/AsynkronIT/protoactor-go/blob/3992780c0af683deb5ec3746f4ec5845139c6e42/actor/props.go#L41)  。  要更改此产生行为，可以使用actor.FromSpawnFunc（）或Props.WithSpawnFunc（）设置替代函数以覆盖默认行为。  如果未设置，则默认使用[actor.DefaultSpawner](https://github.com/AsynkronIT/protoactor-go/blob/3992780c0af683deb5ec3746f4ec5845139c6e42/actor/spawn.go#L13)  。  其行为如下：

-   默认的[spawner](https://github.com/AsynkronIT/protoactor-go/blob/3992780c0af683deb5ec3746f4ec5845139c6e42/actor/local_process.go#L9)创建了一个[actor.localProcess](https://github.com/AsynkronIT/protoactor-go/blob/3992780c0af683deb5ec3746f4ec5845139c6e42/actor/local_process.go#L9)实例，它是一个actor.Process实现。  
    
-   将实例添加到[actor.ProcessRegistry](https://github.com/AsynkronIT/protoactor-go/blob/3992780c0af683deb5ec3746f4ec5845139c6e42/actor/process_registry.go#L62)  。  
    -   如果已经注册了给定的id，注册表将返回错误。  
        
-   创建一个actor.localContext，它是一个actor.Context实现。  这将存储所有上下文数据。  
    
-   为上下文创建邮箱。  要修改邮箱的行为，请使用[Props.WithDispatcher（）](https://github.com/AsynkronIT/protoactor-go/blob/3992780c0af683deb5ec3746f4ec5845139c6e42/actor/props.go#L78)和[Props.WithMailbox（）](https://github.com/AsynkronIT/protoactor-go/blob/3992780c0af683deb5ec3746f4ec5845139c6e42/actor/props.go#L60)  。  
    
-   创建的邮箱存储在actor.localProcess实例中。  
    
-   指向进程的指针设置为actor.PID的字段。  
    
-   actor.localContext还将actor.PID引用为“self”。  
    
-   启动邮箱  
    
-   将mailboxMessage作为系统消息排入邮箱，该消息是actor.Started的实例。  
    

构造完成并且成功启动actor生命周期后，将返回新actor的actor.PID。

# 儿童演员建设

通过引入的actor构造过程，开发人员可以创建任何“root actor”，一个没有父级的actor。  要实现层次化的actor系统，请使用actor.Context的Spawn（）或SpawnNamed（）方法。  这些方法与actor.Spawn（）和actor.SpawnNamed（）的工作方式类似，但最大的区别在于它们在产生actor和新创建的actor之间创建了父子关系。  他们的工作如下：

1.  检查是否设置了Props.guardianStrategy  
    -   如果设置，它会发生恐慌。  因为主叫演员将成为父母并且有义务成为主管，所以不需要设置主持人。  该策略是为第一部分中介绍的自定义监督创建父actor。  
        
2.  调用Props.spawn（）  
    -   该ID的格式为{parent-id} / {child-id}  
        
    -   自己的PID被设置为新actor的父级  
        
3.  将创建的actor actor.PID添加到其子级  
    
4.  开始观察创建的actor.PID以订阅其生命周期事件  
    

[请参阅示例代码。](https://translate.googleusercontent.com/translate_c?depth=1&hl=zh-CN&prev=search&rurl=translate.google.com&sl=en&sp=nmt4&u=https://github.com/AsynkronIT/protoactor-go/tree/3992780c0af683deb5ec3746f4ec5845139c6e42/examples/supervision&xid=17259,15700023,15700124,15700186,15700191,15700201,15700237,15700242,15700248&usg=ALkJrhiQCS3_9qlRalDrC1ju1J3wd01gvQ)

# 主管战略

这是父母演员有责任照顾其儿童演员的特殊状态。  当一个儿童演员不能再控制其状态时，基于“让它崩溃”的理念，儿童演员通过恐慌（）将这种情况通知父母演员。  父actor通过recover（）接收此类通知，并决定如何处理此类失败的actor。  这个决定是由一个可定制的[actor.SupervisorStrategy做出的](https://github.com/AsynkronIT/protoactor-go/blob/3992780c0af683deb5ec3746f4ec5845139c6e42/actor/supervision.go#L9)  。  如果开发人员没有明确设置策略，  [则会](https://github.com/AsynkronIT/protoactor-go/blob/3992780c0af683deb5ec3746f4ec5845139c6e42/actor/supervision.go#L36)在actor构造上设置[actor.defaultSupervisorStrategy](https://github.com/AsynkronIT/protoactor-go/blob/3992780c0af683deb5ec3746f4ec5845139c6e42/actor/supervision.go#L36)  。

监督流程如下：

1.  邮箱通过目标actor上下文的localContext.InvokeUserMessage（）将消息传递给Actor.Recieve（）。  
    
2.  在Actor.Receive（）中，actor调用panic（）。  
    
3.  呼叫者邮箱使用recover（）捕获此类无法控制的状态。  
    
4.  邮箱调用localContext.EscalateFailure（），其中localContext是失败的actor的。  
    1.  在localContext.EscalateFailure（）中，这告诉自己暂停任何传入的消息，直到恢复完成。  
        
    2.  创建actor.Failure实例，其中包含失败原因和其他统计信息，其中“reason”是传递给panic（）的参数。  
        
    3.  判断失败的演员是否有父母  
        -   如果没有找到，则失败的actor是“root actor”，因此actor.Failure被传递给actor.handleRootFactor（）。  
            
        -   如果找到，则将actor.Failure传递给父级的PID.sendSystemMessage（）以通知失败状态  
            1.  该消息已排入父actor的邮箱  
                
            2.  父邮箱调用其localContext.InvokeSystemMessage。  
                
            3.  actor.Failure传递给localContext.handleFailure  
                
            4.  如果它的actor.Actor实体本身实现了actor.SupervisorStrategy，则调用其HandleFailure（）。  
                
            5.  如果没有，则调用其主管实体的handleFailure（）。  
                
            6.  在HandleFailure（）中，决定恢复策略并调用localContext。（ResumeChildren | RestartChildren | StopChildren | EscalateFailure）。  
                

[请参阅示例代码。](https://translate.googleusercontent.com/translate_c?depth=1&hl=zh-CN&prev=search&rurl=translate.google.com&sl=en&sp=nmt4&u=https://github.com/AsynkronIT/protoactor-go/tree/3992780c0af683deb5ec3746f4ec5845139c6e42/examples/supervision&xid=17259,15700023,15700124,15700186,15700191,15700201,15700237,15700242,15700248&usg=ALkJrhiQCS3_9qlRalDrC1ju1J3wd01gvQ)

# 即将到来的界面变化

根据“  [即将推出的设计/ API变更](https://translate.googleusercontent.com/translate_c?depth=1&hl=zh-CN&prev=search&rurl=translate.google.com&sl=en&sp=nmt4&u=https://github.com/AsynkronIT/protoactor-go/issues/239&xid=17259,15700023,15700124,15700186,15700191,15700201,15700237,15700242,15700248&usg=ALkJrhhhv0qbzuPJNQCEFjblpsvwBYXYJA)  ”这一问题，预计会发生巨大的界面变化。

# 进一步阅读

有关更多信息，请参阅以下文章

-   [protoactor-go 101：演员如何相互沟通](https://translate.googleusercontent.com/translate_c?depth=1&hl=zh-CN&prev=search&rurl=translate.google.com&sl=en&sp=nmt4&u=https://blog.oklahome.net/2018/09/protoactor-go-messaging-protocol.html&xid=17259,15700023,15700124,15700186,15700191,15700201,15700237,15700242,15700248&usg=ALkJrhi8UkvQn3XZbbVsJ0UES-xFZBwRQQ)  
    
-   [protoactor-go 101：actor.Future如何同步并发任务执行](https://translate.googleusercontent.com/translate_c?depth=1&hl=zh-CN&prev=search&rurl=translate.google.com&sl=en&sp=nmt4&u=https://blog.oklahome.net/2018/11/protoactor-go-how-future-works.html&xid=17259,15700023,15700124,15700186,15700191,15700201,15700237,15700242,15700248&usg=ALkJrhjsT-EjsssLRFpheUzv-cZruQzsHA)  
    
-   [protoactor-go 201：中间件如何拦截传入和传出的消息](https://translate.googleusercontent.com/translate_c?depth=1&hl=zh-CN&prev=search&rurl=translate.google.com&sl=en&sp=nmt4&u=https://blog.oklahome.net/2018/11/protoactor-go-middleware.html&xid=17259,15700023,15700124,15700186,15700191,15700201,15700237,15700242,15700248&usg=ALkJrhidmsbOqCz5xLgu7T47W9rkUgBD4g)  
    
-   [protoactor-go 201：使用插件向actor添加行为](https://translate.googleusercontent.com/translate_c?depth=1&hl=zh-CN&prev=search&rurl=translate.google.com&sl=en&sp=nmt4&u=https://blog.oklahome.net/2018/12/protoactor-go-use-plugin-to-add-behavior.html&xid=17259,15700023,15700124,15700186,15700191,15700201,15700237,15700242,15700248&usg=ALkJrhg72egWwbxAXI1_IvSb5Gj7-W5jPQ)
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE5MjEwMzA0MjVdfQ==
-->