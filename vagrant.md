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


#
mkdir -p $GOPATH/src/golang.org/x
cd $GOPATH/src/golang.org/x
git clone https://github.com/golang/net.git #这个就是net包
git clone https://github.com/golang/crypto.git #这个就是crypto包
git clone https://github.com/golang/sys.git
git clone https://github.com/golang/mobile.git
git clone https://github.com/golang/text.git
git clone https://github.com/golang/tools.git
git clone https://github.com/golang/image.git
--------------------- 
作者：风.foxwho 
来源：CSDN 
原文：https://blog.csdn.net/fenglailea/article/details/79107124 
版权声明：本文为博主原创文章，转载请附上博文链接！
> Wr
itten with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTEyMzI4MzUzNDEsNDc3MzA0NTc1LDExNz
Y4NzQwNjIsNDI1MDk2NzMwLDE3MDkxMDIxNTYsLTE3NzA2MzQ0
MzIsLTE0MjMxNzM1M119
-->