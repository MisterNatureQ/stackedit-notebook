#!/bin/bash

# Copy hosts info
cat <<EOF > /etc/hosts
127.0.0.1   localhost
127.0.1.1   vagrant.vm  vagrant
192.168.56.11 kube-m1
192.168.56.12 kube-m2
192.168.56.13 kube-m3
192.168.56.14 kube-n1
192.168.56.15 kube-n2
192.168.56.16 kube-n3
# The following lines are desirable for IPv6 capable hosts
::1     localhost ip6-localhost ip6-loopback
ff02::1 ip6-allnodes
ff02::2 ip6-allrouters
EOF

# change time zone
cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
timedatectl set-timezone Asia/Shanghai
# set yum mirror
rm /etc/yum.repos.d/CentOS-Base.repo
cp /vagrant/yum/*.* /etc/yum.repos.d/
mv /etc/yum.repos.d/CentOS7-Base-163.repo /etc/yum.repos.d/CentOS-Base.repo
# install  kmod and ceph-common for rook
yum install -y wget curl conntrack-tools vim net-tools socat ntp kmod ceph-common
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

# Download kubelet and kubectl
KUBE_URL="https://storage.googleapis.com/kubernetes-release/release/v1.10.3/bin/linux/amd64"
#wget "${KUBE_URL}/kubelet" -O /usr/local/bin/kubelet
cp /vagrant/yum/kubelet /usr/local/bin/
chmod +x /usr/local/bin/kubelet

if [[ ${HOSTNAME} =~ m ]]; then
  #wget "${KUBE_URL}/kubectl" -O /usr/local/bin/kubectl
  cp /vagrant/yum/kubectl /usr/local/bin/
  chmod +x /usr/local/bin/kubectl
fi

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
eyJoaXN0b3J5IjpbLTEzOTE4Mjc0ODRdfQ==
-->