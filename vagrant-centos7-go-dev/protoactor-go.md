
##go get list > file.txt

protoactor-go-dev
## 主要的功能是一个事物管理

## internal 内核
protoactor-go-dev/internal/core

## goring 缝以补裆；刺
protoactor-go-dev/internal/queue/goring

## MPSC MultiProtocol Serial Controller 多协议串行控制器 ? MPSC（multi produce single consumer）多生产者单消费者
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




## remote
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

## actor
protoactor-go-dev/actor

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
eyJoaXN0b3J5IjpbMTcyMzczMTU3OCw3Mjk1MTkyMjFdfQ==
-->