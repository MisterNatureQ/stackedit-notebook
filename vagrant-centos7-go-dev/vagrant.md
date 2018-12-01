## 基于 vagrant 的开发环境搭建

需要先安装 virtual box  然后安装  vagrant 


## 安装go环境


## 安装开发环境
sudo yum -y groupinstall "Development tools"


## centos7 关闭防火墙
systemctl status firewalld.service
systemctl stop firewalld.service
systemctl disable firewalld.service#禁止防火墙服务器
wget https://dl.google.com/go/go1.11.2.linux-amd64.tar.gz

tar -C /usr/local -xzf go1.11.2.linux-amd64.tar.gz


#跟新环境变量
source /etc/profile

#国内的go get问题的解决
#安装gopm
go get -u github.com/gpmgo/gopm

#用gopm get -g代替go getgopm get  
#不采用-g参数，会把依赖包下载.vendor目录 
#采用-g 参数，可以把依赖包下载到GOPATH目录中；

gopm get -g golang.org/x/net
gopm get -g google.golang.org/grpc

#go 的依赖包
golang.org/x/net
google.golang.org/grpc


# [golang 包管理工具](https://blog.csdn.net/fenglailea/article/details/79107124)

mkdir -p $GOPATH/src/golang.org/x
cd $GOPATH/src/golang.org/x
git clone https://github.com/golang/net.git #这个就是net包
git clone https://github.com/golang/crypto.git #这个就是crypto包
git clone https://github.com/golang/sys.git
git clone https://github.com/golang/mobile.git
git clone https://github.com/golang/text.git
git clone https://github.com/golang/tools.git
git clone https://github.com/golang/image.git

go get github.com/golang/protobuf
go get github.com/golang/protobuf/protoc-gen-go

所以不能使用go get的方式安装，正确的安装方式：

# [golang安装gRpc](https://www.jianshu.com/p/dba4c7a6d608)
git clone https://github.com/grpc/grpc-go.git $GOPATH/src/google.golang.org/grpc

git clone https://github.com/golang/net.git $GOPATH/src/golang.org/x/net

git clone https://github.com/golang/text.git $GOPATH/src/golang.org/x/text

go get -u github.com/golang/protobuf/{proto,protoc-gen-go}

git clone https://github.com/google/go-genproto.git $GOPATH/src/google.golang.org/genproto

cd $GOPATH/src/

go install google.golang.org/grpc


git clone https://github.com/AsynkronIT/protoactor-go.git $GOPATH/src/github.com/protoactor-go

## 安装protoc
#编译安装protobuf的编译器protoc
wget https://github.com/google/protobuf/releases/download/v3.6.1/protobuf-all-3.6.1.tar.gz
tar zxvf protobuf-all-3.6.1.tar.gz
cd protobuf-all-3.6.1
./autogen.sh
./configure
make
make install


# 执行 protoactor-go make  protoc-gen-gogoslick 报错

[root@localhost protoactor-go]# make
cd actor/; protoc --gogoslick_out=Mgoogle/protobuf/any.proto=github.com/gogo/protobuf/types,plugins=grpc:. --proto_path=. --proto_path=/home/gocode/src ./*.proto
protoc-gen-gogoslick: program not found or is not executable
--gogoslick_out: protoc-gen-gogoslick: Plugin failed with status code 1.

通过安装 updateproto.bat 里面指定的包解决

```
# cat updateproto.bat 
go get -u github.com/gogo/protobuf/proto
go get -u github.com/gogo/protobuf/protoc-gen-gogo
go get -u github.com/gogo/protobuf/gogoproto
go get -u github.com/gogo/protobuf/protoc-gen-gofast
go get -u google.golang.org/grpc
go get -u github.com/gogo/protobuf/protoc-gen-gogofast
go get -u github.com/gogo/protobuf/protoc-gen-gogofaster
go get -u github.com/gogo/protobuf/protoc-gen-gogoslick
```

``
`
go get -u -v github.com/golang/protobuf/protoc-gen-go
go get -u -v github.com/gogo/protobuf/protoc-gen-gofast
```

#执行 
go test `go list ./... | grep -v consul` | grep 'no test files'
缺失的包
go get github.com/stretchr/testify

# 继续make 遇见错误多make几次 

```go
[root@localhost protoactor-go]# go test `go list ./... | grep -v consul` | grep 'ok'           
ok      github.com/AsynkronIT/protoactor-go/actor       (cached)
ok      github.com/AsynkronIT/protoactor-go/cluster/weighted    (cached) [no tests to run]
ok      github.com/AsynkronIT/protoactor-go/eventstream (cached)
ok      github.com/AsynkronIT/protoactor-go/internal/queue/goring       (cached)
ok      github.com/AsynkronIT/protoactor-go/internal/queue/mpsc (cached)
ok      github.com/AsynkronIT/protoactor-go/log (cached)
ok      github.com/AsynkronIT/protoactor-go/mailbox     (cached)
ok      github.com/AsynkronIT/protoactor-go/persistence (cached)
ok      github.com/AsynkronIT/protoactor-go/plugin      (cached)
ok      github.com/AsynkronIT/protoactor-go/remote      (cached)
ok      github.com/AsynkronIT/protoactor-go/router      (cached)
ok      github.com/AsynkronIT/protoactor-go/stream      (cached)
```


## 开发环境


## 多任务系统

-   并发操作必须是引擎可控的
    
    提供并发操作的统一接口
    
-   Actor模式
    
    并发系统，以Actor模式独占鳌头。搜索了下github，使用该库应该足以应对：
    
    

数据持久化方案
使用redis做内存数据库

官方推荐的redis github地址：

https://github.com/go-redis/redis.git

使用mysql做数据持久化

官方推荐的mysql github地址：

https://github.com/go-sql-driver/mysql.git


服务发现机制
zookeeper

该库比较出名，这里不得不罗列下。这里不考虑用它。原因见下面

etcd

使用 Go语言写的，模仿zookeeper的一个开源项目。由于在用Go语言开发，因此选择该库。github地址为：

https://github.com/coreos/etcd.git

NavMesh 构成的世界

目前貌似 go 相关的 NavMesh的资料不是很多。比如在github上搜索下，只有1个 4stars 的go 项目：

> [https://github.com/mrazza/gonav.git](https://github.com/mrazza/gonav.git)

astar寻路

直接上github地址：

> [https://github.com/beefsack/go-astar.git](https://github.com/beefsack/go-astar.git)



### Redis集群 + Mysql分库
这里提倡一种比较理想的数据库部署方案。Redis集群 + Mysql分库

Mysql按ID分段，依次追加

如 1- 100w 为第一个分库；100w - 200w 为第二个分库等等。分库按需追加即可

mysql客户端可以稍作封装，来支持热发现mysql分库；插入数据操作重定向到最后一个分库（数据插入边界问题检查）

Redis集群只做内存数据库

热点数据有redis来分担，做缓存

不用开slave、不用做数据持久化。这样redis对系统资源消耗最小 
redis集群中挂掉一台也只需要重启即可，丢失的仅是缓存数据。

## [Go游戏服务器开发的一些思考](https://blog.csdn.net/u013272009/article/list/3)


### [Eclipse + Golang 开发环境搭建 （要点备忘）](https://blog.csdn.net/u013272009/article/details/72933337)

### [Go游戏服务器开发的一些思考（一）：语言层面](https://blog.csdn.net/u013272009/article/details/72983690)

### [Go游戏服务器开发的一些思考（二）：综合考察（上）](https://blog.csdn.net/u013272009/article/details/73001638)

### [Go游戏服务器开发的一些思考（三）：综合考察（中）](https://blog.csdn.net/u013272009/article/details/73028642)

### [Go游戏服务器开发的一些思考（八）：Docker桥接网络及固定]IP(https://blog.csdn.net/u013272009/article/details/75093888)

### [# Go游戏服务器开发的一些思考（九）：Docker桥接网络及固定IP (二)](https://blog.csdn.net/u013272009/article/details/75315042)

### [# Go游戏服务器开发的一些思考（十）：goroutine和coroutine](https://blog.csdn.net/u013272009/article/details/76273380)

### [# Go游戏服务器开发的一些思考（十一）：IO游戏同步](https://blog.csdn.net/u013272009/article/details/76708056)
> 这里的介绍比较重要


### [[Go游戏服务器开发的一些思考（十二）：行为树behavior3go介绍](https://blog.csdn.net/u013272009/article/details/77131226)](https://blog.csdn.net/u013272009/article/details/77131226)
> 在游戏开发中，以状态切换来驱动其执行流程的系统，引入行为树可以大大简化编码和配置
behavior3go github网址为：[https://github.com/magicsea/behavior3go](https://github.com/magicsea/behavior3go)


### [Go游戏服务器开发的一些思考（十三）：behavior3go的一些坑（备忘）](https://blog.csdn.net/u013272009/article/details/78073587)

### [Go游戏服务器开发的一些思考（十四）：IO游戏同步（二）](https://blog.csdn.net/u013272009/article/details/78082595)
> ### 移动同步类型

### [Go游戏服务器开发的一些思考（十五）：gochart图表制作](https://blog.csdn.net/u013272009/article/details/78208957)

### [Go游戏服务器开发的一些思考（十六）：IO游戏服务器架构](https://blog.csdn.net/u013272009/article/details/78209687)

### [Go游戏服务器开发的一些思考（十七）：IO游戏同步（三）](https://blog.csdn.net/u013272009/article/details/78240679)

### [Go游戏服务器开发的一些思考（十八）：Docker内网环境搭建（备忘）](https://blog.csdn.net/u013272009/article/details/78363537)

### [Go游戏服务器开发的一些思考（十九）：服务器架构之服务发现](https://blog.csdn.net/u013272009/article/details/78373358)

### [Go游戏服务器开发的一些思考（二十）：Docker Swarm部署Etcd示例](https://blog.csdn.net/u013272009/article/details/78430827)

### [Go游戏服务器开发的一些思考（二十一）：Go语言的两处脑残设定](https://blog.csdn.net/u013272009/article/details/78441380)

### [Go游戏服务器开发的一些思考（二十二）：Godep包管理介绍](https://blog.csdn.net/u013272009/article/details/78446118)


### [Go游戏服务器开发的一些思考（二十三）：Go语言Log库封装技巧](https://blog.csdn.net/u013272009/article/details/78463086)

### [goclipse的autoimport（备忘）](https://blog.csdn.net/u013272009/article/details/78483881)

### [Dockerfile脚本示例：Python3+Protobuf+wxPython](https://blog.csdn.net/u013272009/article/details/78492501)

### []()
### []()
### []()
### []()
### []()
### []()
### []()
### []()
### []()
### []()
### []()
### []()
### []()
### []()
### []()
### []()
### []()
### []()
### []()
### []()
### []()
### []()
### []()
### []()
### []()
### []()
### []()
### []()
### []()
### []()
### []()
### []()
### []()
### []()

go get -u github.com/go-redis/redis

<!--stackedit_data:
eyJoaXN0b3J5IjpbLTEwMzYwNzA4NDQsMTU3MTI4NzIyLDY5OD
kzNTc2NCwtMjMxNzUxMTA0LC0xMTQ2NjM3OTIzLC0xMzA0MTk3
NzgzLDE5NzMwMTIyMDgsMTE2Nzk0Njc5LC0xMDQzMDY2NzExLC
01OTUxNjUwODUsLTE3MDczMjAxMDEsLTEwMzc1MDkwNzksLTgw
MDAxODk3OSwxNzM2MTA3NTksMzY0NTkyODU1LDEyMDY1ODQzMT
YsNzQ1NTUzNTI0LDEwNjY3NDQ4MzIsLTEzODYwODI1MDUsLTMw
NzYwNjY1NV19
-->