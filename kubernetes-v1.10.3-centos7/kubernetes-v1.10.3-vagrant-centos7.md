# kubernetes-v1.10.3-vagrant-centos7
	#Kubernetes v1.10.x HA 全手動苦工安裝教學(TL;DR)
	https://kairen.github.io/2018/04/05/kubernetes/deploy/manual-v1.10/
	
	#部署参看
	http://www.cnblogs.com/bbling/p/9112267.html
	http://www.cnblogs.com/bbling/p/9112294.html
	http://www.cnblogs.com/bbling/p/9112304.html
	http://www.cnblogs.com/bbling/p/9112309.html
	http://www.cnblogs.com/bbling/p/9112309.html

	#手册
	http://docs.kubernetes.org.cn/115.html
	https://jimmysong.io/kubernetes-handbook/concepts/taint-and-toleration.html
	https://www.bookstack.cn/read/kubernetes-handbook/guide-tls-bootstrapping.md

	#Kubernetes之Taints与Tolerations 污点和容忍
	https://cloud.tencent.com/info/21f27eb131873f979d6275f085dfabdc.html
	
# 本文主要记录中间遇到的问题 跟多参看上面

## 目录结构
	--yum
		--CentOS7-Base-163.repo
		--kubectl 手动下载 或是 解开setup.sh中的 #wget "${KUBE_URL}/kubectl "
		--kubelet 手动下载 或是 解开setup.sh中的 #wget "${KUBE_URL}/kubelet"
	--setup.sh 
	--Vagrantfile

##  vagrant 打包box

	vagrant package --base my-virtualbox

##  初始环境
	systemctl stop firewalld && systemctl disable firewalld

	### 修改 root 密码登录
	passwd root

	###  修改root 远程登录

	vim /etc/ssh/sshd_config
	#打开
	PasswordAuthentication yes
	PermitRootLogin yes
	#打开免密登录
	PubkeyAuthentication yes
	RSAAuthentication yes
	PasswordAuthentication yes
	#重启ssh
	systemctl restart sshd.service



	ssh-keygen -t rsa
	cd ~/.ssh-copy-id

	ssh-copy-id kube-m1
	ssh-copy-id kube-m2
	ssh-copy-id kube-m3
	ssh-copy-id kube-n1
	ssh-copy-id kube-n2
	ssh-copy-id kube-n3


	#关闭所有节点的 SELinux

	vim /etc/selinux/config
	SELINUX=disabled


	getenforce ##也可以用这个命令检查  查看 SELinux 状态

	注意：如果不关闭 SELinux k8s 挂载目录会报 Permission denied 错误

	#关闭swap
	vim    /etc/fstab  #注释swap


	#开机启动docker
	systemctl enable docker &&  systemctl start docker

	yum install lrzsz

## 批量打包镜像
	导出方便多节点部署
	
	docker save $(docker images | grep -v REPOSITORY | awk 'BEGIN{OFS=":";ORS=" "}{print $1,$2}') -o haha.tar
	#加载镜像
	docker load -i haha.tar

## 下载image
	
	ALI_REP="registry.cn-hangzhou.aliyuncs.com/google_containers"
	GCR="gcr.io/google_containers"
	K8S_GCR="k8s.gcr.io"

	#docker pull quay.io/kubernetes-ingress-controller/nginx-ingress-controller:0.12.0
	docker pull registry.cn-hangzhou.aliyuncs.com/google_containers/nginx-ingress-controller:0.12.0
	docker tag registry.cn-hangzhou.aliyuncs.com/google_containers/nginx-ingress-controller:0.12.0 quay.io/kubernetes-ingress-controller/nginx-ingress-controller:0.12.0

	docker pull quay.io/calico/cni:v2.0.3
	docker pull quay.io/calico/node:v3.0.4

	#"gcr.io/google_containers"

	for IMAGE in k8s-dns-kube-dns-amd64:1.14.7 k8s-dns-dnsmasq-nanny-amd64:1.14.7 k8s-dns-sidecar-amd64:1.14.7 heapster-amd64:v1.5.2 addon-resizer:1.8.1 addon-resizer:1.8.1 heapster-grafana-amd64:v4.4.3 defaultbackend:1.4 heapster-influxdb-amd64:v1.3.3; do
	    echo "--- $IMAGE ---"
	    docker pull $ALI_REP/$IMAGE
	    docker tag $ALI_REP/$IMAGE $GCR/$IMAGE
	    docker rmi $ALI_REP/$IMAGE
	done


	#"k8s.gcr.io"

	for IMAGE in kubernetes-dashboard-amd64:v1.8.3 ; do
	    echo "--- $IMAGE ---"
	    docker pull $ALI_REP/$IMAGE
	    docker tag $ALI_REP/$IMAGE $K8S_GCR/$IMAGE
	    docker rmi $ALI_REP/$IMAGE
	done

	for IMAGE in kube-controller-manager-amd64:v1.10.3 kube-apiserver-amd64:v1.10.3 kube-scheduler-amd64:v1.10.3 etcd-amd64:3.1.13 k8s-dns-kube-dns-amd64:1.14.7 k8s-dns-dnsmasq-nanny-amd64:1.14.7; do
	    echo "--- $IMAGE ---"
	    docker pull $ALI_REP/$IMAGE
	    docker tag $ALI_REP/$IMAGE $GCR/$IMAGE
	    docker rmi $ALI_REP/$IMAGE
	done
	for IMAGE in kube-proxy-amd64:v1.10.3 pause-amd64:3.1; do
	    echo "--- $IMAGE ---"
	    docker pull $ALI_REP/$IMAGE
	    docker tag $ALI_REP/$IMAGE $K8S_GCR/$IMAGE
	    docker rmi $ALI_REP/$IMAGE
	done


## vagrant 操作 

	vagrant up	启动本地环境
	vagrant halt	关闭本地环境
	vagrant suspend	暂停本地环境
	vagrant resume	恢复本地环境
	vagrant reload	修改了 Vagrantfile 后，使之生效（相当于先 halt，再 up）
	vagrant ssh	通过 ssh 登录本地环境所在虚拟机
	vagrant destroy	彻底移除本地环境


## 部署 Master
	本部分将说明如何建立与设定 Kubernetes Master 角色，过程中会部署以下元件：

	kube-apiserver：提供 REST APIs，包含授权、认证与状态储存等。
	kube-controller-manager：负责维护集群的状态，如自动扩展，滚动更新等。
	kube-scheduler：负责资源排程，依据预定的排程策略将 Pod 分配到对应节点上。
	Etcd：储存集群所有状态的 Key/Value 储存系统。
	HAProxy：提供负载平衡器。
	Keepalived：提供虚拟网络位址 (VIP)。   -- 192.168.56.10   对外访问的节点
	Calico架构 Calico 创建和管理一个扁平的三层网络(不需要overlay)，每个容器会分配一个可路由的ip。由于通信时不需要解包和封包，网络性能损耗小，易于排查，且易于水平扩展。


## 部署中的问题

	wget https://pkg.cfssl.org/R1.2/cfssl_linux-amd64 && chmod +x cfssl_linux-amd64 && mv cfssl_linux-amd64 /usr/local/bin/cfssl
	wget https://pkg.cfssl.org/R1.2/cfssljson_linux-amd64 && chmod +x cfssljson_linux-amd64 && mv cfssljson_linux-amd64 /usr/local/bin/cfssljson
	wget https://pkg.cfssl.org/R1.2/cfssl-certinfo_linux-amd64 && chmod +x cfssl-certinfo_linux-amd64 && mv cfssl-certinfo_linux-amd64 /usr/local/bin/cfssl-certinfo

---	
	head -c 32 /dev/urandom | base64
	cat <<EOF > /etc/kubernetes/encryption.yml
	kind: EncryptionConfig
	apiVersion: v1
	resources:
	  - resources:
	      - secrets
	    providers:
	      - aescbc:
	          keys:
	            - name: key1
	              secret: pNZL8xMD7cDxXbJ6SJrPhfJiQxeUuItiPNMKvrgf8mI=
	      - identity: {}
	EOF

---
	kubernetes 的虚拟IP的概念 192.168.56.10

---
	calicoctl node status
	kubectl get nodes -o wide
	kubectl get pods --all-namespaces -o wide

	docker network ls
	docker network xxxx
	docker ps
	route -n

	kubectl get deployments --all-namespaces

### Deployment的一些基础命令
	$ kubectl describe deployments  #查询详细信息，获取升级进度
	$ kubectl rollout pause deployment/nginx-deployment2  #暂停升级
	$ kubectl rollout resume deployment/nginx-deployment2  #继续升级
	$ kubectl rollout undo deployment/nginx-deployment2  #升级回滚
	$ kubectl scale deployment nginx-deployment --replicas 10  #弹性伸缩Pod数量


### 查看docker 网络
	docker network inspect bridge

### 服务操作命令
	systemctl enable kubelet.service 
	systemctl start kubelet.service
	systemctl stop kubelet.service
	systemctl disable kubelet.service

	rm -rf /var/lib/kubelet /var/log/kubernetes /var/lib/etcd
	mkdir -p /var/lib/kubelet /var/log/kubernetes /var/lib/etcd

	systemctl enable kubelet.service && systemctl start kubelet.service


重启之后必须关闭swap 

### 开机启动docker 
	vagrant ssh kube-m1
	su
	systemctl enable docker &&  systemctl start docker


kubectl get nodes

### certificate signing requests  证书签名请求
	kubectl get csr 
	kubectl get po,svc --all-namespaces
	kubectl get clusterrolebindings --all-namespaces
	kubectl get roles --all-namespaces



	kubectl -n kube-system get po,svc -l k8s-app=kubernetes-dashboard
	kubectl -n kube-system logs -f kube-scheduler-kube-m1
	kubectl -n kube-system get po,svc
	kubectl -n kube-system get po -l k8s-app=kube-dns
	kubectl -n kube-system describe po -l k8s-app=kube-dns
	kubectl -n kube-system delete po -l k8s-app=kube-dns

	kubectl -n kube-system describe po -l k8s-app=kubernetes-dashboard


## RABC 配置详解
	https://jimmysong.io/kubernetes-handbook/concepts/rbac.html

	kubectl describe no kube-m1
	kubectl describe no kube-m2
	kubectl describe no kube-m3
	kubectl describe no kube-n1
	kubectl describe no kube-n2
	kubectl describe no kube-n3


## ingress 不能布置的问题 非master 节点配置了 taint
	#https://cloud.tencent.com/info/21f27eb131873f979d6275f085dfabdc.html 参看文章

	[root@kube-m1 ~]# kubectl taint nodes node-role.kubernetes.io/master="":NoSchedule --all
	Node kube-m1 already has node-role.kubernetes.io/master taint(s) with same effect(s) and --overwrite is false
	Node kube-m2 already has node-role.kubernetes.io/master taint(s) with same effect(s) and --overwrite is false
	Node kube-m3 already has node-role.kubernetes.io/master taint(s) with same effect(s) and --overwrite is false
	Node kube-n1 already has node-role.kubernetes.io/master taint(s) with same effect(s) and --overwrite is false
	Node kube-n2 already has node-role.kubernetes.io/master taint(s) with same effect(s) and --overwrite is false
	Node kube-n3 already has node-role.kubernetes.io/master taint(s) with same effect(s) and --overwrite is false

	[root@kube-m1 ~]# kubectl taint nodes node-role.kubernetes.io/master="":NoExecute --all
	node "kube-m1" tainted
	node "kube-m2" tainted
	node "kube-m3" tainted
	node "kube-n1" tainted
	node "kube-n2" tainted
	node "kube-n3" tainted


	kubectl taint node [node] key=value[effect]   
	      其中[effect] 可取值： [ NoSchedule | PreferNoSchedule | NoExecute ]
	       NoSchedule ：一定不能被调度。
	       PreferNoSchedule：尽量不要调度。
	       NoExecute：不仅不会调度，还会驱逐Node上已有的Pod。
	  示例：kubectl taint node 10.3.1.16 test=16:NoSchedule

	kubectl taint node [node] key=value[effect]   
	      其中[effect] 可取值： [ NoSchedule | PreferNoSchedule | NoExecute ]
	       NoSchedule ：一定不能被调度。
	       PreferNoSchedule：尽量不要调度。
	       NoExecute：不仅不会调度，还会驱逐Node上已有的Pod。
	  示例：kubectl taint node 10.3.1.16 test=16:NoSchedule
	  
	  
	kubectl taint node kube-n1 node-role.kubernetes.io/master:NoExecute-
	kubectl taint node kube-n2 node-role.kubernetes.io/master:NoExecute-
	kubectl taint node kube-n3 node-role.kubernetes.io/master:NoExecute-

	kubectl taint node kube-n1 node-role.kubernetes.io/master:NoSchedule-
	kubectl taint node kube-n2 node-role.kubernetes.io/master:NoSchedule-
	kubectl taint node kube-n3 node-role.kubernetes.io/master:NoSchedule-

	kubectl taint node kube-m1 node-role.kubernetes.io/master:NoExecute-
	kubectl taint node kube-m2 node-role.kubernetes.io/master:NoExecute-
	kubectl taint node kube-m3 node-role.kubernetes.io/master:NoExecute-

	kubectl taint node kube-m1 node-role.kubernetes.io/master="":NoSchedule
	kubectl taint node kube-m2 node-role.kubernetes.io/master="":NoSchedule
	kubectl taint node kube-m3 node-role.kubernetes.io/master="":NoSchedule

	kubectl taint nodes node-role.kubernetes.io/master="":NoSchedule --all


	kubectl taint node kube-m3 node-role.kubernetes.io/master="":NoSchedule


	kubectl -n ingress-nginx get po,svc
	kubectl -n ingress-nginx describe role nginx-ingress-role
	kubectl -n ingress-nginx describe ServiceAccount  nginx-ingress-serviceaccount
	kubectl -n ingress-nginx describe ClusterRole nginx-ingress-clusterrole
	kubectl -n kube-system describe ClusterRole cluster-admin

	kubectl -n ingress-nginx describe po default-http-backend
	kubectl -n ingress-nginx describe po nginx-ingress-controller
	kubectl -n kube-system describe po etcd-kube-m1


	kubectl -n ingress-nginx delete po default-http-backend
	kubectl -n ingress-nginx delete po nginx-ingress-controller

	kubectl -n ingress-nginx delete svc default-http-backend
	kubectl -n ingress-nginx delete svc ingress-nginx
	kubectl delete ns ingress-nginx


## 访问测试 ingress
	curl 192.168.56.10 -H 'Host: test.nginx.com'



## Helm Tiller Server

### helm手册
	https://whmzsu.github.io/helm-doc-zh-cn/architecture-zh_cn.html

yum install -y socat

gcr.io/kubernetes-helm/tiller:v2.8.1
### 需要下载的iamge

	docker pull registry.cn-hangzhou.aliyuncs.com/kube_containers/tiller:v2.9.1
	docker tag registry.cn-hangzhou.aliyuncs.com/kube_containers/tiller:v2.9.1 gcr.io/kubernetes-helm/tiller:v2.9.1
	docker rmi registry.cn-hangzhou.aliyuncs.com/kube_containers/tiller:v2.9.1


kubectl -n kube-system get po -l app=helm
helm version

### 删除 移除helm init创建的目录等数据
	helm reset --remove-helm-home
	helm init --service-account tiller

	#使用阿里云的源
	helm init --service-account tiller --stable-repo-url https://aliacs-app-catalog.oss-cn-hangzhou.aliyuncs.com/charts/
	helm repo add incubator https://aliacs-app-catalog.oss-cn-hangzhou.aliyuncs.com/charts-incubator/
	helm repo update

	-rwxr-xr-x 755
	chmod 755 helm

	#dashboard-amd64 token
	eyJhbGciOiJSUzI1NiIsImtpZCI6IiJ9.eyJpc3MiOiJrdWJlcm5ldGVzL3NlcnZpY2VhY2NvdW50Iiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9uYW1lc3BhY2UiOiJrdWJlLXN5c3RlbSIsImt1YmVybmV0ZXMuaW8vc2VydmljZWFjY291bnQvc2VjcmV0Lm5hbWUiOiJkYXNoYm9hcmQtdG9rZW4tNTg4Z3giLCJrdWJlcm5ldGVzLmlvL3NlcnZpY2VhY2NvdW50L3NlcnZpY2UtYWNjb3VudC5uYW1lIjoiZGFzaGJvYXJkIiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9zZXJ2aWNlLWFjY291bnQudWlkIjoiZGIwZGQyMmItNjg5NC0xMWU4LWEwODktNTI1NDAwYWQzYjQzIiwic3ViIjoic3lzdGVtOnNlcnZpY2VhY2NvdW50Omt1YmUtc3lzdGVtOmRhc2hib2FyZCJ9.Dy3fr1MPXNLVtwSq0CzvCHMBRb_aZ0U5-KdRohdwifreZtPgOOvR9ScSsp1dP1Av8W_bP1M2nz_QSprAYViTpcvwm5kufo03rN5bgtF-itw-Pv34M5_mlHH-cbhwPG061wYlE1l79CBAiIbGpeqAfW2WqoKLRwROciJ7qlkLLnnci2CMoqJc2QH3BbHP2zodnd7Jzxsm7hcXTqI3Z97yxsVBY56gWts6z4V2-K_LD8sjGlbGfDA9turMHLk8FPRKhropU4IZvsbyW7AkX_poCQX5JvxaRJIV7pFp945SaOM6eUJfUIX57KcotFhVwj0YNOnnJuYgWAbH44oQWN1J8g

	kubectl get deployment --all-namespaces
	kubectl get svc  --all-namespaces
	kubectl get pod  -o wide  --all-namespaces
	kubectl -n kube-system get po -l app=helm



## dashboard 入口
	https://192.168.56.10:6443/api/v1/namespaces/kube-system/services/https:kubernetes-dashboard:/proxy/

## ingress 部署的问题

	https://blog.csdn.net/weiguang1017/article/details/77102845
	kubectl create ns ingress-nginx
	kubectl delete -f xxx.yaml
	kubectl -n kube-system delete po -l app=helm



## Heapster 比较占用资源 先关闭
	Heapster 是 Kubernetes 社區維護的容器叢集監控與效能分析工具。Heapster 會從 Kubernetes apiserver 取得所有 Node 資訊，然後再透過這些 Node 來取得 kubelet 上的資料，最後再將所有收集到資料送到 Heapster 的後台儲存 InfluxDB，最後利用 Grafana 來抓取 InfluxDB 的資料源來進行視覺化。

	在k8s-m1透過 kubectl 來建立 kubernetes monitor 即可：

	$ kubectl apply -f "https://kairen.github.io/files/manual-v1.10/addon/kube-monitor.yml.conf"
	$ kubectl -n kube-system get po,svc
	NAME                                           READY     STATUS    RESTARTS   AGE
	...
	po/heapster-74fb5c8cdc-62xzc                   4/4       Running   0          7m
	po/influxdb-grafana-55bd7df44-nw4nc            2/2       Running   0          7m

	NAME                       TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)             AGE
	...
	svc/heapster               ClusterIP   10.100.242.225   <none>        80/TCP              7m
	svc/monitoring-grafana     ClusterIP   10.101.106.180   <none>        80/TCP              7m
	svc/monitoring-influxdb    ClusterIP   10.109.245.142   <none>        8083/TCP,8086/TCP   7m
	···
	完成後，就可以透過瀏覽器存取 Grafana Dashboard。



## kubeapps   中间有很多资源需要 google  ing
	
	docker pull registry.cn-hangzhou.aliyuncs.com/crstudio/tiller:v2.8.0
	docker tag registry.cn-hangzhou.aliyuncs.com/crstudio/tiller:v2.8.0 gcr.io/kubernetes-helm/tiller:v2.8.0
	docker rmi registry.cn-hangzhou.aliyuncs.com/crstudio/tiller:v2.8.0

	https://hub.kubeapps.com/
	#阿里云 帮助文档
	https://help.aliyun.com/document_detail/70098.html?spm=a2c4g.11186623.6.581.FFiDyH

	#kubeapps 安装
	https://github.com/kubeapps/kubeapps/blob/master/docs/getting-started.md

	#显示 yaml
	kubeapps up --dry-run -o yaml
	kubectl get all --all-namespaces -l created-by=kubeapps


	eyJhbGciOiJSUzI1NiIsImtpZCI6IiJ9.eyJpc3MiOiJrdWJlcm5ldGVzL3NlcnZpY2VhY2NvdW50Iiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9uYW1lc3BhY2UiOiJkZWZhdWx0Iiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9zZWNyZXQubmFtZSI6Imt1YmVhcHBzLW9wZXJhdG9yLXRva2VuLTQ0dDVjIiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9zZXJ2aWNlLWFjY291bnQubmFtZSI6Imt1YmVhcHBzLW9wZXJhdG9yIiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9zZXJ2aWNlLWFjY291bnQudWlkIjoiMDhkMTFlM2UtNmEzMi0xMWU4LTk1N2MtNTI1NDAwYWQzYjQzIiwic3ViIjoic3lzdGVtOnNlcnZpY2VhY2NvdW50OmRlZmF1bHQ6a3ViZWFwcHMtb3BlcmF0b3IifQ.x5h-iDc4PhsXhPP6Td2AznTfdYY-HYn1JmMwOjMKSWfkl9QtwM7WlCc7YVxjdJweV2teLIzvh-vnGjstoLiBryVyf_hBaVlxzgboaqgV4XWsJjzfDA0DQdfnVOVlIL7nz0VoTCVTJgHe_NFmCC2wW6x2v-LUFI8QWdT3bXk38GN0H-A-k1J9dfOsiIvFPVVcluRPwsJNjq7Jz__Omoj7ZOH_20ul0q5nvyF8amoulEYA_Sn57y3y9Osbc7XwMpU5v_SA9s76YfWwq89DDYAqf27vkUpnU1sMIiTNmc2U-GrVqVfUEgnnniC37K4858NzSXONhL-KEefXUQuSue3TKg

	#需要创建PersistentVolume(PV)


	kind: PersistentVolume
	apiVersion: v1
	metadata:
	  name: PV-hostPath
	  labels:
	    release: stable
	spec:
	  capacity:
	    storage: 8Gi
	  accessModes:
	    - ReadWriteOnce
	  persistentVolumeReclaimPolicy: Recycle
	  hostPath:
	    path: /tmp/data
		

	[root@kube-m1 ~]# kubeapps up --dry-run -o yaml > kubeapps.yaml	
	[root@kube-m1 ~]# cat kubeapps.yaml | grep image:
	docker pull kubeless/function-image-builder:v0.6.0
	docker pull quay.io/bitnami/sealed-secrets-controller:v0.5.1
	docker pull kubeapps/apprepository-controller:v1.0.0-alpha.3
	docker pull kubeapps/chartsvc:v1.0.0-alpha.3
	docker pull bitnami/nginx:1.12
	docker pull kubeapps/dashboard:v1.0.0-alpha.3
	docker pull bitnami/mongodb:3.4.9-r1
	docker pull gcr.io/kubernetes-helm/tiller:v2.8.0
	docker pull bitnami/helm-crd-controller:v0.3.0
	docker pull bitnami/kubeless-controller-manager:v0.6.0
			
	#下载完镜像后，替换镜像拉取策略		
	sed -i 's#Always#IfNotPresent#g' kubeapps.yaml		
		
		
# TODO 动态调整节点的 个数 减少机器压力
	目前 3个master 必须有两个 在线 整体才能稳定 且master 需要 2核2G node需要1核1G
	
	
	
# Kubernetes高级实践：Master高可用方案设计和踩过的那些坑
	https://yq.aliyun.com/articles/79615?t=t1
	
	
# kubernetes在腾讯游戏的应用实践
	https://www.kubernetes.org.cn/1423.html
	
	
# ceph
	### Kubernetes 集成 Ceph 后端存储教程
	https://blog.csdn.net/shida_csdn/article/details/78579043
	
	### Ceph 离线安装教程
	https://blog.csdn.net/shida_csdn/article/details/78266110
	
	### ceph工作原理及安装
	https://www.jianshu.com/p/25163032f57f
	
	### 在Kubernetes集群中用Helm托管安装Ceph集群并提供后端存储
	https://www.kubernetes.org.cn/3896.html
	
	### ceph 中文手册
	http://docs.ceph.org.cn/start/intro/
	
	
# 如何获取gcr.io上的镜像	
	https://blog.csdn.net/qq_27028561/article/details/79064414



> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE1OTc2NTc1MTZdfQ==
-->