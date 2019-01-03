
#!/bin/bash

# Copy hosts info


# change time zone
cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
timedatectl set-timezone Asia/Shanghai
# set yum mirror
rm /etc/yum.repos.d/CentOS-Base.repo
cp /vagrant/yum/*.* /etc/yum.repos.d/
mv /etc/yum.repos.d/CentOS7-Base-163.repo /etc/yum.repos.d/CentOS-Base.repo

yum install -y wget

#install go 

wget https://dl.google.com/go/go1.11.2.linux-amd64.tar.gz
tar -C /usr/local -xzf go1.11.2.linux-amd64.tar.gz

sudo mkdir -p /home/gocode

sudo echo 'export GOROOT=/usr/local/go' >> /etc/profile
sudo echo 'export GOPATH=/home/gocode' >> /etc/profile
sudo echo 'export PATH=$PATH:$GOROOT/bin:$GOPATH/bin' >> /etc/profile


#使环境变量生效 这里没有这个命令
source "/etc/profile"

#使环境变量生效
export "GOROOT=/usr/local/go"
export "GOPATH=/home/gocode"
export "PATH=$PATH:$GOROOT/bin:$GOPATH/bin"

GOPATH="/home/gocode"
GOROOT="/usr/local/go"
sudo mkdir -p "${GOPATH}/src/golang.org/x"



# 常用工具
# 在CentOS 7上启用epel版本 才能安装 htop 
sudo yum -y install epel-release
sudo yum -y groupinstall "Development tools"
sudo yum install -y lrzsz htop


# install  kmod and ceph-common for rook
sudo yum install -y wget curl conntrack-tools vim net-tools socat ntp kmod ceph-common lrzsz


# install docker








## centos7 关闭防火墙
sudo systemctl status firewalld.service
sudo systemctl stop firewalld.service
sudo systemctl disable firewalld.service#禁止防火墙服务器

#下载需要的包

sudo mkdir -p ${GOPATH}/src/golang.org/x
cd "${GOPATH}/src/golang.org/x"
git clone https://github.com/golang/net.git "${GOPATH}/src/golang.org/x/net"
git clone https://github.com/golang/crypto.git "${GOPATH}/src/golang.org/x/crypto"
git clone https://github.com/golang/sys.git "${GOPATH}/src/golang.org/x/sys"
git clone https://github.com/golang/mobile.git "${GOPATH}/src/golang.org/x/mobile"
git clone https://github.com/golang/text.git "${GOPATH}/src/golang.org/x/text"
git clone https://github.com/golang/tools.git "${GOPATH}/src/golang.org/x/tools"
git clone https://github.com/golang/image.git "${GOPATH}/src/golang.org/x/image"
git clone https://github.com/golang/oauth2.git "${GOPATH}/src/golang.org/x/oauth2"
git clone https://github.com/grpc/grpc-go.git "${GOPATH}/src/google.golang.org/grpc"
git clone https://github.com/google/go-genproto.git "${GOPATH}/src/google.golang.org/genproto"
git clone https://github.com/AsynkronIT/protoactor-go.git "${GOPATH}/src/github.com/protoactor-go"
cd "${GOPATH}/src/github.com/AsynkronIT/protoactor-go"
go get -v ./...

# grpc
go get -u github.com/golang/protobuf/{proto,protoc-gen-go}
go install google.golang.org/grpc

sudo "${GOROOT}/bin/go" install google.golang.org/grpc
sudo "${GOROOT}/bin/go" get -u github.com/golang/protobuf/{proto,protoc-gen-go}

#sudo go get ./...
sudo "${GOROOT}/bin/go" get ./...
#sudo go test `go list ./... | grep -v consul` | grep 'ok' 
sudo "${GOROOT}/bin/go" test `go list ./... | grep -v consul` | grep 'ok' 

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
eyJoaXN0b3J5IjpbLTExOTgyNDIxNTcsMzM0MjE3OTIxLDE2MT
AyNTYwNDAsLTQyNTE5ODY4OSwxNzA4MTE2MzUzLDQ2MTMzNjc0
OCwtMTI4MTc1MjU0NCwtNjk1MDA4Mjg3LC0yMzMxOTg4MjcsMz
g5NjAxNTQ1XX0=
-->