# 基于 vagrant 的开发环境搭建

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
./autogen.sh
./configure
make
make install




#执行 
go test `go list ./... | grep -v consul` | grep 'no test files'
缺失的包
go get github.com/stretchr/testify


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


> Wr
itten with [StackEdit](https://stackedit.io/)
.




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

# Go游戏服务器开发的一些思考（八）：Docker桥接网络及固定IP


go get -u github.com/go-redis/redis

<!--stackedit_data:
eyJoaXN0b3J5IjpbNDYzNTE2MywtMTAzNzUwOTA3OSwtODAwMD
E4OTc5LDE3MzYxMDc1OSwzNjQ1OTI4NTUsMTIwNjU4NDMxNiw3
NDU1NTM1MjQsMTA2Njc0NDgzMiwtMTM4NjA4MjUwNSwtMzA3Nj
A2NjU1XX0=
-->