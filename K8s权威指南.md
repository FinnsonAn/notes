

# K8s是什么
Kubernetes 是一个可移植的、可扩展的开源平台，用于管理容器化的工作负载和服务，可促进声明式配置和自动化。 Kubernetes 拥有一个庞大且快速增长的生态系统。Kubernetes 的服务、支持和工具广泛可用。

Kubernetes 这个名字源于希腊语，意为“舵手”或“飞行员”。k8s 这个缩写是因为 k 和 s 之间有八个字符的关系。 Google 在 2014 年开源了 Kubernetes 项目。Kubernetes 建立在 Google 在大规模运行生产工作负载方面拥有十几年的经验 的基础上，结合了社区中最好的想法和实践。



# 为什么要用K8s

# 发展历史
![img](https://d33wubrfki0l68.cloudfront.net/26a177ede4d7b032362289c6fccd448fc4a91174/eb693/images/docs/container_evolution.svg)

## 1. 传统部署时代 资源即服务IAAS(Infrastructure as a service)

早期，各个组织机构在物理服务器上运行应用程序。无法为物理服务器中的应用程序定义资源边界，这会导致资源分配问题。 

例如，如果在物理服务器上运行多个应用程序，则可能会出现一个应用程序占用大部分资源的情况， 结果可能导致其他应用程序的性能下降。 一种解决方案是在不同的物理服务器上运行每个应用程序，但是由于资源利用不足而无法扩展， 并且维护许多物理服务器的成本很高。

缺点：每个服务器都要安装所需要的环境，如数据库、nginx、jdk等，不同服务器上配置也可能会存在不一致
## 2. 虚拟化部署时代：

作为解决方案，引入了虚拟化。虚拟化技术允许你在单个物理服务器的 CPU 上运行多个虚拟机（VM）。 虚拟化允许应用程序在 VM 之间隔离，并提供一定程度的安全，因为一个应用程序的信息 不能被另一应用程序随意访问。

虚拟化技术能够更好地利用物理服务器上的资源，并且因为可轻松地添加或更新应用程序 而可以实现更好的可伸缩性，降低硬件成本等等。

每个 VM 是一台完整的计算机，在虚拟化硬件之上运行所有组件，包括其自己的操作系统。

## 3. 容器部署时代：

容器类似于 VM，但是它们具有被放宽的隔离属性，可以在应用程序之间共享操作系统（OS）。 因此，容器被认为是轻量级的。容器与 VM 类似，具有自己的文件系统、CPU、内存、进程空间等。 由于它们与基础架构分离，因此可以跨云和 OS 发行版本进行移植。

容器因具有许多优势而变得流行起来。下面列出的是容器的一些好处：

敏捷应用程序的创建和部署：与使用 VM 镜像相比，提高了容器镜像创建的简便性和效率。
持续开发、集成和部署：通过快速简单的回滚（由于镜像不可变性），支持可靠且频繁的 容器镜像构建和部署。
关注开发与运维的分离：在构建/发布时而不是在部署时创建应用程序容器镜像， 从而将应用程序与基础架构分离。
跨开发、测试和生产的环境一致性：在便携式计算机上与在云中相同地运行。
跨云和操作系统发行版本的可移植性：可在 Ubuntu、RHEL、CoreOS、本地、 Google Kubernetes Engine 和其他任何地方运行。
松散耦合、分布式、弹性、解放的微服务：应用程序被分解成较小的独立部分， 并且可以动态部署和管理 - 而不是在一台大型单机上整体运行。
资源隔离：可预测的应用程序性能。
资源利用：高效率和高密度。


缺点：随着docker增加，维护和监控docker繁琐，如果某个docker挂了需要立即部署一个新的，如果请求暴增需要再部署一堆docker，相应的负载均衡相关也要更新

例如我需要部署一个JavaWeb项目，用到4台物理机，A、B、C、D，A服务器部署Nginx、B、C部署Tomcat、D服务器部署MySQL服务器，他们之间的网络联通通过基本的TCP就可以访问。但是如果一旦容器化之后，例如我在A服务器上部署Docker-Nginx并将容器例如80端口映射到物理机80端口、B、C服务器上部署Docker-Tomcat并将容器例如8080端口映射到物理机8080端口、D服务器上部署MySQL服务器并将端口3306端口映射到物理机3306端口，并且我们一般容器化就是最大化利用资源，一台物理机上会部署多个Docker容器，如果某个docker挂了需要立即部署一个新的，如果请求暴增需要再部署一堆docker，你会发现仅端口管理就让人头大，更不用说集群的容错恢复等


## 4. docker swarm
Docker公司也意识到这个问题，在2014年12月的 DockerCon上发布Swarm,但是Docker公司在Docker开源项目的发展上，始终保持着绝对的权威和发言权，并在多个场合用实际行动挑战到了其他玩家（比如，CoreOS、RedHat，甚至谷歌和微软）的切身利益。
Google、RedHat等开源基础设施领域玩家们，共同牵头发起了一个名为CNCF（Cloud Native Computing Foundation）的基金会。这个基金会的目的其实很容易理解：它希望，以Kubernetes项目为基础，建立一个由开源基础设施领域厂商主导的、按照独立基金会方式运营的平台级社区，来对抗以Docker公司为核心的容器商业生态

## 4. K8s 登场

容器是打包和运行应用程序的好方式。在生产环境中，你需要管理运行应用程序的容器，并确保不会停机。 例如，如果一个容器发生故障，则需要启动另一个容器。Kubernetes 为你提供了一个可弹性运行分布式系统的框架。 Kubernetes 会满足你的扩展要求、故障转移、部署模式等

- 服务发现和负载均衡
Kubernetes 可以使用 DNS 名称或自己的 IP 地址公开容器，如果进入容器的流量很大， Kubernetes 可以负载均衡并分配网络流量，从而使部署稳定。
- 存储编排
Kubernetes 允许你自动挂载你选择的存储系统，例如本地存储、公共云提供商等。
- 自动部署和回滚
你可以使用 Kubernetes 描述已部署容器的所需状态，它可以以受控的速率将实际状态 更改为期望状态。例如，你可以自动化 Kubernetes 来为你的部署创建新容器， 删除现有容器并将它们的所有资源用于新容器。
- 自动完成装箱计算
Kubernetes 允许你指定每个容器所需 CPU 和内存（RAM）。 当容器指定了资源请求时，Kubernetes 可以做出更好的决策来管理容器的资源。
- 自我修复
Kubernetes 重新启动失败的容器、替换容器、杀死不响应用户定义的 运行状况检查的容器，并且在准备好服务之前不将其通告给客户端



## docker swarm vs K8s

![img](https://pic2.zhimg.com/80/v2-6e72e3a6179f63824211d64b7a2ef381_720w.jpg)

# K8s 的组成

一个 Kubernetes 集群由一组被称作节点的机器组成。这些节点上运行 Kubernetes 所管理的容器化应用。集群具有至少一个工作节点。
这张图表展示了包含所有相互关联组件的 Kubernetes 集群
![img](https://d33wubrfki0l68.cloudfront.net/2475489eaf20163ec0f54ddc1d92aa8d4c87c96b/e7c81/images/docs/components-of-kubernetes.svg)

## 控制平面组件（Control Plane Components）一般存在于control plane node/ master node

### kube-apiserver
API 服务器是 Kubernetes 控制面的组件， 该组件公开了 Kubernetes API。 API 服务器是 Kubernetes 控制面的前端。

Kubernetes API 服务器的主要实现是 kube-apiserver。 

### etcd
etcd 是兼具一致性和高可用性的键值数据库，可以作为保存 Kubernetes 所有集群数据的后台数据库。

### kube-scheduler
控制平面组件，负责监视新创建的、未指定运行节点（node）的 Pods，选择节点让 Pod 在上面运行。
调度决策考虑的因素包括单个 Pod 和 Pod 集合的资源需求、硬件/软件/策略约束、亲和性和反亲和性规范、数据位置、工作负载间的干扰和最后时限。


### kube-controller-manager
运行控制器进程的控制平面组件。
- 节点控制器（Node Controller）: 负责在节点出现故障时进行通知和响应
- 任务控制器（Job controller）: 监测代表一次性任务的 Job 对象，然后创建 Pods 来运行这些任务直至完成
- 端点控制器（Endpoints Controller）: 填充端点(Endpoints)对象(即加入 Service 与 Pod)
- 服务帐户和令牌控制器（Service Account & Token Controllers）: 为新的命名空间创建默认帐户和 API 访问令牌


### cloud-controller-manager
云控制器管理器使得你可以将你的集群连接到云提供商的 API 之上


## Node 组件 
节点组件在每个节点上运行，维护运行的 Pod 并提供 Kubernetes 运行环境。

### kubelet
一个在集群中每个节点（node）上运行的代理。 它保证容器（containers）都 运行在 Pod 中。
### kube-proxy
kube-proxy 是集群中每个节点上运行的网络代理， 实现 Kubernetes 服务（Service） 概念的一部分。
kube-proxy 维护节点上的网络规则。这些网络规则允许从集群内部或外部的网络会话与 Pod 进行网络通信。

### 容器运行时（Container Runtime） 
容器运行环境是负责运行容器的软件。你需要在集群内每个节点上安装一个 容器运行时 以使 Pod 可以运行在上面。
Kubernetes 支持多个容器运行环境: Docker、 containerd、CRI-O 以及任何实现 Kubernetes CRI (容器运行环境接口)。

### Pods
Pod 是可以在 Kubernetes 中创建和管理的、最小的可部署的计算单元。
Pod （就像在鲸鱼荚或者豌豆荚中）是一组（一个或多个） 容器； 这些容器共享存储、网络、以及怎样运行这些容器的声明。 Pod 中的内容总是并置（colocated）的并且一同调度，在共享的上下文中运行。 Pod 所建模的是特定于应用的“逻辑主机”，其中包含一个或多个应用容器， 这些容器是相对紧密的耦合在一起的。 Pod 不是进程，而是容器运行的环境。
Kubernetes 集群中的 Pod 主要有两种用法：
- 运行单个容器的 Pod。"每个 Pod 一个容器"模型是最常见的 Kubernetes 用例； 在这种情况下，可以将 Pod 看作单个容器的包装器，并且 Kubernetes 直接管理 Pod，而不是容器。
- 运行多个协同工作的容器的 Pod。 Pod 可能封装由多个紧密耦合且需要共享资源的共处容器组成的应用程序。 只有在一些场景中，容器之间紧密关联时你才应该使用这种模式。

## [高可用拓扑选项](https://kubernetes.io/zh/docs/setup/production-environment/tools/kubeadm/ha-topology/)
堆叠（Stacked） etcd 拓扑
etcd 分布式数据存储集群堆叠在 kubeadm 管理的控制平面节点上，作为控制平面的一个组件运行。
![img](https://d33wubrfki0l68.cloudfront.net/d1411cded83856552f37911eb4522d9887ca4e83/b94b2/images/kubeadm/kubeadm-ha-topology-stacked-etcd.svg)
外部 etcd 拓扑
etcd 分布式数据存储集群在独立于控制平面节点的其他节点上运行。但是，具有此拓扑的 HA 集群至少需要三个用于控制平面节点的主机和三个用于 etcd 节点的主机。
![img](https://d33wubrfki0l68.cloudfront.net/ad49fffce42d5a35ae0d0cc1186b97209d86b99c/5a6ae/images/kubeadm/kubeadm-ha-topology-external-etcd.svg)


# Minikube DEMO




## [install minikube](https://minikube.sigs.k8s.io/docs/start/)
基础设施: virtualbox
1. 以windows为例子 下载安装文件 https://storage.googleapis.com/minikube/releases/latest/minikube-installer.exe
2. Start your cluster
```bash
minikube start
```
3. 与集群交互
```bash
# 如果已经安装过kubectl 执行如下命令访问你的集群
kubectl get po -A
# 如果没有安装过kubectl 执行如下命令下载
# minikube kubectl -- <kubectl commands>
minikube kubectl -- get po -A
# print

C:\Users\shaan>minikube kubectl -- get pods -A
    > kubectl.exe.sha256: 64 B / 64 B [----------------------] 100.00% ? p/s 0s
    > kubectl.exe: 45.63 MiB / 45.63 MiB [----------] 100.00% 8.16 MiB p/s 5.8s
NAMESPACE     NAME                               READY   STATUS    RESTARTS   AGE
kube-system   coredns-558bd4d5db-ssfrj           1/1     Running   0          2m50s
kube-system   etcd-minikube                      1/1     Running   0          2m58s
kube-system   kube-apiserver-minikube            1/1     Running   0          2m58s
kube-system   kube-controller-manager-minikube   1/1     Running   0          2m58s
kube-system   kube-proxy-nzv5d                   1/1     Running   0          2m50s
kube-system   kube-scheduler-minikube            1/1     Running   0          2m58s
kube-system   storage-provisioner                1/1     Running   1          3m3s

# 打开dashboard
minikube dashboard
```

## DEMO1 集群动态更新

### 创建
配置文件结构如下

    apiVersion: apps/v1
    kind: Deployment
    metadata:
      name: nginx-deployment
      labels:
        app: nginx
    spec:
      replicas: 3
      selector:
        matchLabels:
          app: nginx
      template:
        metadata:
          labels:
            app: nginx
        spec:
          containers:
          - name: nginx
            image: nginx:1.14.2
            ports:
            - containerPort: 80


```bash
# 创建deployment 
minikube kubectl -- apply -f https://k8s.io/examples/controllers/nginx-deployment.yaml
# 运行 kubectl get deployments 检查 Deployment 是否已创建
minikube kubectl -- get deployments
```
    NAME               DESIRED   CURRENT   UP-TO-DATE   AVAILABLE   AGE
    nginx-deployment   3         0         0            0           1s

```bash
# 查看Pods
minikube kubectl -- get pods

```

    NAME                                READY   STATUS             RESTARTS   AGE
    nginx-deployment-66b6c48dd5-l2dlc   1/1     Running            0          32s
    nginx-deployment-66b6c48dd5-wrfmx   1/1     Running            0          32s
    nginx-deployment-66b6c48dd5-xtbhb   1/1     Running            0          32s

    - NAME 列出了集群中 Deployment 的名称。
    - READY 显示应用程序的可用的 副本 数。显示的模式是“就绪个数/期望个数”。
    - UP-TO-DATE 显示为了达到期望状态已经更新的副本数。
    - AVAILABLE 显示应用可供用户使用的副本数。
    - AGE 显示应用程序运行的时间。

### 更新

```bash
# 更新镜像版本
minikube kubectl -- set image deployment/nginx-deployment nginx=nginx:1.16.1 --record
deployment.apps/nginx-deployment image updated
```

```bash
# 查看Pods
minikube kubectl -- get pods

```
    NAME                                READY   STATUS             RESTARTS   AGE
    nginx-deployment-559d658b74-5mcpz   1/1     Running            0          2m5s
    nginx-deployment-559d658b74-g4jqd   1/1     Running            0          108s
    nginx-deployment-559d658b74-n6x75   1/1     Running            0          110s

```bash
# 获取deployment 更多信息
minikube kubectl -- describe deployments
```

    Name:                   nginx-deployment
    Namespace:              default
    CreationTimestamp:      Fri, 27 Aug 2021 19:02:42 +0800
    Labels:                 app=nginx
    Annotations:            deployment.kubernetes.io/revision: 2
                            kubernetes.io/change-cause: kubectl.exe set image deployment/nginx-deployment nginx=nginx:1.16.1 --cluster=minikube --record=true
    Selector:               app=nginx
    Replicas:               3 desired | 3 updated | 3 total | 3 available | 0 unavailable
    StrategyType:           RollingUpdate
    MinReadySeconds:        0
    RollingUpdateStrategy:  25% max unavailable, 25% max surge
    Pod Template:
      Labels:  app=nginx
      Containers:
      nginx:
        Image:        nginx:1.16.1
        Port:         80/TCP
        Host Port:    0/TCP
        Environment:  <none>
        Mounts:       <none>
      Volumes:        <none>
    Conditions:
      Type           Status  Reason
      ----           ------  ------
      Available      True    MinimumReplicasAvailable
      Progressing    True    NewReplicaSetAvailable
    OldReplicaSets:  <none>
    NewReplicaSet:   nginx-deployment-559d658b74 (3/3 replicas created)
    Events:
      Type    Reason             Age    From                   Message
      ----    ------             ----   ----                   -------
      Normal  ScalingReplicaSet  14m    deployment-controller  Scaled up replica set nginx-deployment-66b6c48dd5 to 3
      Normal  ScalingReplicaSet  4m11s  deployment-controller  Scaled up replica set nginx-deployment-559d658b74 to 1
      Normal  ScalingReplicaSet  3m56s  deployment-controller  Scaled down replica set nginx-deployment-66b6c48dd5 to 2
      Normal  ScalingReplicaSet  3m56s  deployment-controller  Scaled up replica set nginx-deployment-559d658b74 to 2
      Normal  ScalingReplicaSet  3m54s  deployment-controller  Scaled down replica set nginx-deployment-66b6c48dd5 to 1
      Normal  ScalingReplicaSet  3m54s  deployment-controller  Scaled up replica set nginx-deployment-559d658b74 to 3
      Normal  ScalingReplicaSet  3m52s  deployment-controller  Scaled down replica set nginx-deployment-66b6c48dd5 to 0

可以看到，当第一次创建 Deployment 时，它创建了一个 ReplicaSet（nginx-deployment-66b6c48dd5） 并将其直接扩容至 3 个副本。
更新 Deployment 时，它创建了一个新的 ReplicaSet （nginx-deployment-559d658b74），并将其扩容为 1，然后将旧 ReplicaSet 缩容到 2， 以便至少有 2 个 Pod 可用且最多创建 4 个 Pod。
 然后，它使用相同的滚动更新策略继续对新的 ReplicaSet 扩容并对旧的 ReplicaSet 缩容。 最后，你将有 3 个可用的副本在新的 ReplicaSet 中，旧 ReplicaSet 将缩容到 0。

金丝雀部署
如果要使用 Deployment 向用户子集或服务器子集上线版本，则可以遵循 资源管理 所描述的金丝雀模式，创建多个 Deployment，每个版本一个。


参考：

[一起学习k8s](https://zhuanlan.zhihu.com/p/99397148)

[openstack，docker，mesos，k8s什么关系？](https://www.zhihu.com/question/62985699)





# 安装K8s

## 准备环境

```bash
# centos7 默认开启防火墙服务（firewalld） 而K8s的Master与工作Node之间会有大量的网络通信，安全的做法是再防火墙配置各个组件需要通信的端口号
sudo systemctl disable firewalld
sudo systemctl stop firewalld
# 禁用SELinux，让容器可以读取主机文件系统
sudo setenforce 0
# 允许远程登陆
vi /etc/ssh/sshd_config
systemctl restart sshd
reboot
```

## 安装docker

```bash

# 安装docker
# 设置仓库
sudo yum install -y yum-utils \
  device-mapper-persistent-data \
  lvm2
  
sudo yum-config-manager \
    --add-repo \
    http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
# 安装 Docker Engine-Community
sudo yum install docker-ce docker-ce-cli containerd.io
# 开机启动
systemctl enable docker && systemctl start docker

```



## 使用kubeadm工具快速安装K8s集群

```bash
# 配置yum源
cd /etc/yum.repos.d
sudo vi CentOS-Base.repo

# 添加如下内容
[kuebrnetes]
name=Kubernetes Repository
baseurl=http://mirrors.aliyun.com/kubernetes/yum/repos/kubernetes-el7-x86_64/
enabled=1
gpgcheck=0

# 安装
yum install -y kubelet kubeadm kubectl --disableexcludes=kubernetes

# 开机启动
systemctl enable kubelet && systemctl start kubelet

# 创建工作空间 
sudo mkdir /workspace
# 将默认参数打印到文件 方便以后参照修改
kubeadm config print init-defaults > init.default.yaml
```



# 使用vagrant k8s box

```bash
# https://app.vagrantup.com/contiv/boxes/k8s-centos
vagrant init contiv/k8s-centos
vagrant up

# login user-name:root password:vagrant

# 允许远程登陆
vi /etc/ssh/sshd_config
systemctl restart sshd
```



