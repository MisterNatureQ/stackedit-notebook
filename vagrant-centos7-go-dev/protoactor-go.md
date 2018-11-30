
##go get list > file.txt



#protoactor-go-dev 当前的原始文件基于 dev 分支

## 主要的功能是一个事物管理

## internal 内核 IdentifyPanic 用于分析Panic 
protoactor-go-dev/internal/core
使用了 
	["runtime"](https://studygolang.com/static/pkgdoc/pkg/runtime.htm)

	
	
# 圆形缓冲区适合实现先进先出缓冲区，而非圆形缓冲区适合后进先出缓冲区 ??	
	
## go ring 环形缓冲区
protoactor-go-dev/internal/queue/goring
	环形缓冲区
	
	
## MPSC（multi produce single consumer）多生产者单消费者 注意这里的 Pop 只能在一个 goroutine 中调用
protoactor-go-dev/internal/queue/mpsc
实现了一个队列先进先出
使用了 	
	["sync/atomic"](https://studygolang.com/static/pkgdoc/pkg/sync_atomic.htm)
		atomic包提供了底层的原子级内存操作，对于同步算法的实现很有用。
		这些函数必须谨慎地保证正确使用。除了某些特殊的底层应用，使用通道或者sync包的函数/类型实现同步更好。
		应通过通信来共享内存，而不通过共享内存实现通信	
			
	["unsafe"](https://studygolang.com/static/pkgdoc/pkg/unsafe.htm)
		unsafe包提供了一些跳过go语言类型安全限制的操作。
		
这两个包

prev := (*node)(atomic.SwapPointer((*unsafe.Pointer)(unsafe.Pointer(&q.head)), unsafe.Pointer(n)))
next := (*node)(atomic.LoadPointer((*unsafe.Pointer)(unsafe.Pointer(&tail.next))))
next := (*node)(atomic.LoadPointer((*unsafe.Pointer)(unsafe.Pointer(&tail.next))))


## actor
protoactor-go-dev/actor
[并发之痛 Thread，Goroutine，Actor](https://studygolang.com/articles/6250)

Actor 模型
Actor 对没接触过这个概念的人可能不太好理解，Actor 的概念其实和 OO 里的对象类似，是一种抽象。面对对象编程对现实的抽象是对象 = 属性 + 行为（method），但当使用方调用对象行为（method）的时候，其实占用的是调用方的 CPU 时间片，是否并发也是由调用方决定的。这个抽象其实和现实世界是有差异的。现实世界更像 Actor 的抽象，互相都是通过异步消息通信的。比如你对一个美女 say hi，美女是否回应，如何回应是由美女自己决定的，运行在美女自己的大脑里，并不会占用发送者的大脑。

所以 Actor 有以下特征：
Processing – actor 可以做计算的，不需要占用调用方的 CPU 时间片，并发策略也是由自己决定。
Storage – actor 可以保存状态
Communication – actor 之间可以通过发送消息通讯

Actor 遵循以下规则：
发送消息给其他的 Actor
创建其他的 Actor
接受并处理消息，修改自己的状态

Actor 的目标：
Actor 可独立更新，实现热升级。因为 Actor 互相之间没有直接的耦合，是相对独立的实体，可能实现热升级。
无缝弥合本地和远程调用 因为 Actor 使用基于消息的通讯机制，无论是和本地的 Actor，还是远程 Actor 交互，都是通过消息，这样就弥合了本地和远程的差异。
容错 Actor 之间的通信是异步的，发送方只管发送，不关心超时以及错误，这些都由框架层和独立的错误处理机制接管。
易扩展，天然分布式 因为 Actor 的通信机制弥合了本地和远程调用，本地Actor 处理不过来的时候，可以在远程节点上启动 Actor 然后转发消息过去。



### protoactor-go-dev\actor\actor.go
actor 模式的实现
Producer 创建 Actor


### protoactor-go-dev\actor\local_context.go
### protoactor-go-dev\actor\local_context_test.go

### protoactor-go-dev\actor\process.go
process 方法 接口
// A Process is an interface that defines the base contract for interaction of actors
// 方法 Process 是定义 actors 参与者 交互的 基本契约 的接口
type Process interface {
	SendUserMessage(pid *PID, message interface{})
	SendSystemMessage(pid *PID, message interface{})
	Stop(pid *PID)
}

### protoactor-go-dev\actor\local_process.go
// 本地方法 local_process  这里实现了 process 的方法
type localProcess struct {
	mailbox mailbox.Inbound
	dead    int32
}

### protoactor-go-dev\actor\process_registry.go
// ProcessRegistry 注册方法

[cmap A "thread" safe map of type string:Anything. ]("github.com/orcaman/concurrent-map")

### protoactor-go-dev\actor\process_registry_test.go


### protoactor-go-dev\actor\pid.go
pid 进程? 方法 标识符 唯一标识
// ref 引用 获取 方法 Process
func (pid *PID) ref() Process

### protoactor-go-dev\actor\pidset.go
### protoactor-go-dev\actor\pidset_test.go
### protoactor-go-dev\actor\pid_test.go

	
### protoactor-go-dev\actor\context.go
context 上下文 语境 环境 接口类型
```go

//Context contains contextual information for actors
//Context 上下文包含 actors 参与者 的上下文信息
type Context interface {
	// Watch registers the actor as a monitor for the specified PID
	// Watch 将 actor 参与者 注册为指定PID的监视器
	Watch(pid *PID)

	// Unwatch unregisters the actor as a monitor for the specified PID
	// Unwatch将 actor 参与者 注销为指定PID的监视器
	Unwatch(pid *PID)

	// Message returns the current message to be processed
	// Message返回要处理的当前消息
	Message() interface{}

	// Sender returns the PID of actor that sent currently processed message
	// Sender 返回 发送当前已处理消息的 actor 参与者 的PID
	Sender() *PID

	//MessageHeader returns the meta information for the currently processed message
	// MessageHeader 返回当前处理的消息的 元信息
	MessageHeader() ReadonlyMessageHeader

	//Tell sends a message to the given PID
	Tell(pid *PID, message interface{})

	//Forward forwards current message to the given PID
	//Forward 将当前消息转发到给定的PID
	Forward(pid *PID)

	//Request sends a message to the given PID and also provides a Sender PID
	//Request 向给定PID发送消息，并提供发送方PID
	Request(pid *PID, message interface{})

	// RequestFuture sends a message to a given PID and returns a Future
	// RequestFuture 向给定PID发送消息并返回 Future
	RequestFuture(pid *PID, message interface{}, timeout time.Duration) *Future

	// SetReceiveTimeout sets the inactivity timeout, after which a ReceiveTimeout message will be sent to the actor.
	// SetReceiveTimeout 设置不活动超时，在此之后将向 actor 参与者发送 接收超时消息 ReceiveTimeout。
	// A duration of less than 1ms will disable the inactivity timer.
	// 持续时间小于1毫秒将禁用不活动计时器。
	//
	// If a message is received before the duration d, the timer will be reset. If the message conforms to
	// the NotInfluenceReceiveTimeout interface, the timer will not be reset
	// 如果在持续时间d之前收到消息，计时器将被重置。
	// 如果消息符合 Not Influence Receive Timeout 没有收到超时 接口，则计时器将不会重置
	SetReceiveTimeout(d time.Duration)

	// ReceiveTimeout returns the current timeout
	ReceiveTimeout() time.Duration

	// SetBehavior replaces the actors current behavior stack with the new behavior
	// SetBehavior 用新行为替换参与者当前的行为堆栈
	SetBehavior(behavior ActorFunc)

	// PushBehavior pushes the current behavior on the stack and sets the current Receive handler to the new behavior
	// PushBehavior 将当前行为推入堆栈，并将当前接收处理程序设置为新行为
	PushBehavior(behavior ActorFunc)

	// PopBehavior reverts to the previous Receive handler
	// 返回到前一个接收处理程序
	PopBehavior()

	// Self returns the PID for the current actor
	Self() *PID

	// Parent returns the PID for the current actors parent
	Parent() *PID

	// Spawn starts a new child actor based on props and named with a unique id
	// Spawn 产物  根据 Props 启动新的子角色，并使用惟一id命名
	Spawn(props *Props) *PID

	// SpawnPrefix starts a new child actor based on props and named using a prefix followed by a unique id
	// SpawnPrefix 根据 Props 启动新的 子角色 child actor，并使用 prefix 前缀 和惟一id进行命名
	SpawnPrefix(props *Props, prefix string) *PID

	// SpawnNamed starts a new child actor based on props and named using the specified name
	// SpawnNamed 基于 Props 道具 启动新的 子角色，并使用指定的 名称 进行命名
	//
	// ErrNameExists will be returned if id already exists
	SpawnNamed(props *Props, id string) (*PID, error)

	// Returns a slice of the actors children
	Children() []*PID

	// Stash stashes the current message on a stack for reprocessing when the actor restarts
	// Stash 将当前消息存储在堆栈中，以便在 actor 参与者 重新启动时进行重新处理
	Stash()

	// Respond sends a response to the to the current `Sender`
	// Respond 向当前的“发送方”发送响应
	//
	// If the Sender is nil, the actor will panic
	// 如果发送方为nil，则 actor 将 panic
	Respond(response interface{})

	// Actor returns the actor associated with this context
	// Actor 返回与此 context 上下文 关联的参与者
	Actor() Actor

	// AwaitFuture 等待 未来
	AwaitFuture(f *Future, continuation func(res interface{}, err error))
}
```
### protoactor-go-dev\actor\props.go
props 道具

```go
// Props represents configuration to define how an actor should be created
// Props 道具 表示 配置定义 应该如何创建 actor 参与者
type Props struct {
	// 生产者 生产者
	actorProducer       Producer
	// mailbox 生产者
	mailboxProducer     mailbox.Producer
	// 守护者 策略
	guardianStrategy    SupervisorStrategy
	// 监管 策略
	supervisionStrategy SupervisorStrategy
	// 入站的中间件
	inboundMiddleware   []InboundMiddleware
	// 出站中间件
	outboundMiddleware  []OutboundMiddleware
	// 调度员 分配器
	dispatcher          mailbox.Dispatcher
	//  产卵；大量生产
	spawner             SpawnFunc
}
```

### protoactor-go-dev\actor\context_example_setBehavior_test.go
### protoactor-go-dev\actor\context_example_setReceiveTimeout_test.go

### protoactor-go-dev\actor\actor_example_test.go
### protoactor-go-dev\actor\behaviorstack.go
### protoactor-go-dev\actor\behaviorstack_test.go
### protoactor-go-dev\actor\behavior_test.go
### protoactor-go-dev\actor\build.bat
### protoactor-go-dev\actor\child_restart_stats.go
### protoactor-go-dev\actor\child_test.go
### protoactor-go-dev\actor\common_test.go


### protoactor-go-dev\actor\deadletter.go
### protoactor-go-dev\actor\deadletter_test.go
### protoactor-go-dev\actor\directive.go
### protoactor-go-dev\actor\directive_string.go
### protoactor-go-dev\actor\doc.go
### protoactor-go-dev\actor\future.go
### protoactor-go-dev\actor\future_example_test.go
### protoactor-go-dev\actor\future_test.go
### protoactor-go-dev\actor\guardian.go
### protoactor-go-dev\actor\interaction_test.go
### protoactor-go-dev\actor\lifecycle_test.go

### protoactor-go-dev\actor\log.go
### protoactor-go-dev\actor\mailbox.go
### protoactor-go-dev\actor\messages.go
### protoactor-go-dev\actor\message_envelope.go
消息封装
### protoactor-go-dev\actor\message_envelope_test.go
### protoactor-go-dev\actor\message_producer.go
### protoactor-go-dev\actor\middleware
### protoactor-go-dev\actor\middleware_chain.go
### protoactor-go-dev\actor\middleware_chain_test.go
### protoactor-go-dev\actor\options.go





### protoactor-go-dev\actor\protos.pb.go
### protoactor-go-dev\actor\protos.proto
### protoactor-go-dev\actor\root_supervisor.go
### protoactor-go-dev\actor\spawn.go
### protoactor-go-dev\actor\spawn_example_test.go
### protoactor-go-dev\actor\spawn_named_example_test.go
### protoactor-go-dev\actor\spawn_test.go
### protoactor-go-dev\actor\strategy_all_for_one.go
### protoactor-go-dev\actor\strategy_exponential_backoff.go
### protoactor-go-dev\actor\strategy_exponential_backoff_test.go
### protoactor-go-dev\actor\strategy_one_for_one.go
### protoactor-go-dev\actor\strategy_one_for_one_test.go
### protoactor-go-dev\actor\strategy_restarting.go
### protoactor-go-dev\actor\supervision.go
### protoactor-go-dev\actor\supervision_event.go
### protoactor-go-dev\actor\supervision_test.go
### protoactor-go-dev\actor\middleware\logging.go
### protoactor-go-dev\actor\middleware\protozip
### protoactor-go-dev\actor\middleware\protozip\inbound_middleware.go
### protoactor-go-dev\actor\middleware\protozip\outbound_middleware.go





## remote 远程
protoactor-go-dev/remote

## router
protoactor-go-dev/router

## stream
protoactor-go-dev/stream

## log
protoactor-go-dev/log

## mailbox
protoactor-go-dev/mailbox

## persistence 坚持 毅力 持久性
protoactor-go-dev/persistence

## plugin
protoactor-go-dev/plugin



## 命令行接口(Command Line Interface)
protoactor-go-dev/cli

## actor
protoactor-go-dev/actor/middleware
protoactor-go-dev/actor/middleware/protozip


## cluster 集群
protoactor-go-dev/cluster

## consul 领事
protoactor-go-dev/cluster/consul

## weighted 加权
protoactor-go-dev/cluster/weighted


## protobuf
protoactor-go-dev/protobuf/protoc-gen-csgrain
protoactor-go-dev/protobuf/protoc-gen-gograin

## event stream 事件流
protoactor-go-dev/eventstream


## examples
protoactor-go-dev/examples

### 反压力
protoactor-go-dev/examples/backpressure
### 聊天
protoactor-go-dev/examples/chat/client
protoactor-go-dev/examples/chat/messages
protoactor-go-dev/examples/chat/server
### 群集
protoactor-go-dev/examples/cluster/member
protoactor-go-dev/examples/cluster/seed
protoactor-go-dev/examples/cluster/shared
### 集群指标
protoactor-go-dev/examples/cluster-metrics/member
protoactor-go-dev/examples/cluster-metrics/seed
protoactor-go-dev/examples/cluster-metrics/shared
### distributed channels
protoactor-go-dev/examples/distributedchannels/messages
protoactor-go-dev/examples/distributedchannels/node1
protoactor-go-dev/examples/distributedchannels/node2
### helloworld
protoactor-go-dev/examples/helloworld
### in process benchmark 过程中基准
protoactor-go-dev/examples/inprocessbenchmark
### life cycle events 生命周期事件
protoactor-go-dev/examples/lifecycleevents
### limit concurrency 限制并发
protoactor-go-dev/examples/limitconcurrency
### mailbox stats 邮箱统计数据
protoactor-go-dev/examples/mailboxstats
### mixins多态
protoactor-go-dev/examples/mixins
### persistence 持久性
protoactor-go-dev/examples/persistence
### receive pipeline 接收管道
protoactor-go-dev/examples/receivepipeline
### receive timeout 接收超时
protoactor-go-dev/examples/receivetimeout
### remote activate 远程激活
protoactor-go-dev/examples/remoteactivate/messages
protoactor-go-dev/examples/remoteactivate/node1
protoactor-go-dev/examples/remoteactivate/node2
### remote benchmark 远程基准
protoactor-go-dev/examples/remotebenchmark/messages
protoactor-go-dev/examples/remotebenchmark/node1
protoactor-go-dev/examples/remotebenchmark/node2
### remote latency 远程延迟
protoactor-go-dev/examples/remotelatency/messages
protoactor-go-dev/examples/remotelatency/node1
protoactor-go-dev/examples/remotelatency/node2
### remote routing 远程路由
protoactor-go-dev/examples/remoterouting/client
protoactor-go-dev/examples/remoterouting/messages
protoactor-go-dev/examples/remoterouting/server
### remote watch 远程监测 远程监视
protoactor-go-dev/examples/remotewatch/messages
protoactor-go-dev/examples/remotewatch/node1
protoactor-go-dev/examples/remotewatch/node2
### request response请求应答 请求回应 请求响应
protoactor-go-dev/examples/requestresponse
### routing 路由 路由 布线
protoactor-go-dev/examples/routing
### set behavior 集合行为
protoactor-go-dev/examples/setbehavior
### spawn benchmark 产生基准
protoactor-go-dev/examples/spawnbenchmark
### supervision 监督监理监管监视
protoactor-go-dev/examples/supervision



<!--stackedit_data:
eyJoaXN0b3J5IjpbLTgyMTIyMDgwOCwxMTU1MzQ0NjYyLDE3Mj
M3MzE1NzgsNzI5NTE5MjIxXX0=
-->