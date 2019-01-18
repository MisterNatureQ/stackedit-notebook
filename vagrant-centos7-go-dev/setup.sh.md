

#!/bin/bash

# Copy hosts info


# change time zone
cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
timedatectl set-timezone Asia/Shanghai

# set yum mirror
rm /etc/yum.repos.d/CentOS-Base.repo
cp /vagrant/yum/*.* /etc/yum.repos.d/
mv /etc/yum.repos.d/CentOS7-Base-163.repo /etc/yum.repos.d/CentOS-Base.repo

yum update -y
yum install -y wget git

# install go 
cd /home/vagrant
cp /vagrant/yum/go1.11.2.linux-amd64.tar.gz /home/vagrant
#wget https://dl.google.com/go/go1.11.2.linux-amd64.tar.gz
tar -C /usr/local -xzf go1.11.2.linux-amd64.tar.gz

# install prototbuf
cd /home/vagrant
cp /vagrant/yum/protobuf-all-3.6.1.tar.gz /home/vagrant
tar -zxvf protobuf-all-3.6.1.tar.gz
cd protobuf-all-3.6.1
#如果使用的不是源码，而是release版本 (已经包含gmock和configure脚本)，可以略过这一步
sudo ./autogen.sh
#指定安装路径
sudo ./configure --prefix=/usr/local/protobuf
#编译
make
#测试，这一步很耗时间
#make check
make install
#(1) vim /etc/profile，添加
export PATH=$PATH:/usr/local/protobuf/bin/
export PKG_CONFIG_PATH=/usr/local/protobuf/lib/pkgconfig/
#保存执行，source /etc/profile。同时在~/.profile中添加上面两行代码，否则会出现登录用户找不到protoc命令。

#(2) 配置动态链接库
#　　vim /etc/ld.so.conf，在文件中添加/usr/local/protobuf/lib（注意: 在新行处添加），然后执行命令: ldconfig




# install consul
cp /vagrant/yum/unzip consul_1.4.0_linux_amd64.zip /home/vagrant
unzip consul_1.4.0_linux_amd64.zip
mv consul /usr/local/bin/

sudo mkdir -p /home/gocode

sudo echo 'export GOROOT=/usr/local/go' >> /etc/profile
sudo echo 'export GOPATH=/home/gocode' >> /etc/profile
sudo echo 'export PATH=$PATH:$GOROOT/bin:$GOPATH/bin' >> /etc/profile

# 常用工具
# 在CentOS 7上启用epel版本 才能安装 htop 
sudo yum -y install epel-release
sudo yum -y groupinstall "Development tools"
#yum groups mark install -y "Development tools"
sudo yum install -y lrzsz htop


# install  kmod and ceph-common for rook
sudo yum install -y wget curl conntrack-tools vim net-tools socat ntp kmod ceph-common lrzsz


# install docker
groupadd docker
usermod -aG docker vagrant
rm -rf ~/.docker/
yum install -y docker.x86_64
cat > /etc/docker/daemon.json <<EOF
{
  "registry-mirrors" : ["https://k64bpq6l.mirror.aliyuncs.com"]
}
EOF

#启动并加入开机启动
sudo systemctl start docker
sudo systemctl enable docker


# install redis
yum install -y redis 
sudo service redis start
sudo chkconfig redis on

# centos7 关闭防火墙
sudo systemctl status firewalld.service
sudo systemctl stop firewalld.service
sudo systemctl disable firewalld.service#禁止防火墙服务器

# telnet
yum -y install telnet-server.x86_64
yum -y install telnet.x86_64
yum -y install xinetd.x86_64
#配置并启动Telnet，xinetd和telnet必须设置开机启动，否则无法启动Telnet服务、
systemctl enable xinetd.service
systemctl enable telnet.socket
#接下来启动服务
systemctl start telnet.socket
systemctl start xinetd
#配置防火墙规则
#firewall-cmd --permanent --add-port=23/tcp
#firewall-cmd --reload
#默认Root无法远程访问，因此需要修改以下配置文件/etc/securetty
#vi /etc/securetty
#在末尾添加
#pts/0
#pts/1

# 使环境变量生效
export "GOROOT=/usr/local/go"
export "GOPATH=/home/gocode"
export "PATH=$PATH:$GOROOT/bin:$GOPATH/bin"
GOPATH="/home/gocode"
GOROOT="/usr/local/go"

#使环境变量生效 这里没有这个命令
source "/etc/profile"


# 下载需要的包
#sudo mkdir -p "${GOPATH}/src/golang.org/x"
sudo git clone https://github.com/golang/net.git "${GOPATH}/src/golang.org/x/net"
sudo git clone https://github.com/golang/crypto.git "${GOPATH}/src/golang.org/x/crypto"
sudo git clone https://github.com/golang/sys.git "${GOPATH}/src/golang.org/x/sys"
sudo git clone https://github.com/golang/mobile.git "${GOPATH}/src/golang.org/x/mobile"
sudo git clone https://github.com/golang/text.git "${GOPATH}/src/golang.org/x/text"
sudo git clone https://github.com/golang/tools.git "${GOPATH}/src/golang.org/x/tools"
sudo git clone https://github.com/golang/image.git "${GOPATH}/src/golang.org/x/image"
sudo git clone https://github.com/golang/oauth2.git "${GOPATH}/src/golang.org/x/oauth2"
sudo git clone https://github.com/grpc/grpc-go.git "${GOPATH}/src/google.golang.org/grpc"
sudo git clone https://github.com/google/go-genproto.git "${GOPATH}/src/google.golang.org/genproto"

# somepackage
go get github.com/stretchr/testify


# grpc
go get -u github.com/golang/protobuf/{proto,protoc-gen-go}
go install google.golang.org/grpc
sudo "${GOROOT}/bin/go" install google.golang.org/grpc
sudo "${GOROOT}/bin/go" get -u github.com/golang/protobuf/{proto,protoc-gen-go}



# go-xserver
sudo go get -v github.com/fananchong/go-xserver


# 可以改成自己改版的 
sudo go get -v github.com/AsynkronIT/protoactor-go
sudo git clone https://github.com/AsynkronIT/protoactor-go.git "${GOPATH}/src/github.com/AsynkronIT/protoactor-go"


# protoactor-go
sudo cd "${GOPATH}/src/github.com/AsynkronIT/protoactor-go"
sudo go get -v ./...
#sudo go get ./...
sudo "${GOROOT}/bin/go" get ./...
#sudo go test `go list ./... | grep -v consul` | grep 'ok' 
sudo "${GOROOT}/bin/go" test `go list ./... | grep -v consul` | grep 'ok' 
go test `go list ./... | grep -v consul` | grep -v 'no test files'

# Setup system vars
cat <<EOF > /etc/sysctl.d/k8s.conf
net.ipv4.ip_forward = 1
net.bridge.bridge-nf-call-ip6tables = 1
net.bridge.bridge-nf-call-iptables = 1
EOF

sysctl -p /etc/sysctl.d/k8s.conf

swapoff -a && sysctl -w vm.swappiness=0
sed '/vagrant--vg-swap_1/d' -i  /etc/fstab
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTI0NDI0NjA4MSwtMTg0ODAzMTY0NywxND
E4OTE1MTA4LC0yMDQyOTQzNjM5LC0xMTk4MjQyMTU3LDMzNDIx
NzkyMSwxNjEwMjU2MDQwLC00MjUxOTg2ODksMTcwODExNjM1My
w0NjEzMzY3NDgsLTEyODE3NTI1NDQsLTY5NTAwODI4NywtMjMz
MTk4ODI3LDM4OTYwMTU0NV19
-->