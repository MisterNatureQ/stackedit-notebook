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


<!--stackedit_data:
eyJoaXN0b3J5IjpbMTIwNjU4NDMxNiw3NDU1NTM1MjQsMTA2Nj
c0NDgzMiwtMTM4NjA4MjUwNSwtMzA3NjA2NjU1XX0=
-->