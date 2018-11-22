基于 vagrant 的开发环境搭建

#安装go环境


#安装开发环境
sudo yum groupinstall "Development tools"


#centos7 关闭防火墙
systemctl status firewalld.service
systemctl stop firewalld.service
systemctl disable firewalld.service，禁止防火墙服务器
wget https://dl.google.com/go/go1.11.2.linux-amd64.tar.gz

tar -C /usr/local -xzf go1.11.2.linux-amd64.tar.gz


#安装go环境



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

> Wr
itten with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTExNDMwMjQyNCwxOTM4OTkwOTAwLC0xNz
Q4NTI2OTUsMjgzNzE4NjkxLC0xMDYxNjYyODEsLTEyNTE1Mzk1
MjUsLTEyNTM0NzE4NzEsLTE5NjUxOTM2NDMsNDc3MzA0NTc1LD
ExNzY4NzQwNjIsNDI1MDk2NzMwLDE3MDkxMDIxNTYsLTE3NzA2
MzQ0MzIsLTE0MjMxNzM1M119
-->