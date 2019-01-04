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









<!--stackedit_data:
eyJoaXN0b3J5IjpbMTM0ODU2MjgyOV19
-->