# 第一部分 K8S基本概念

## 1、k8s概述和特性

* 容器化集群管理系统
* 使用k8s进行容器化应用部署
* 利于应用扩展

## 2、 k8s架构组件

* Master（主控接点）和node（工作接点）
* Master组件
  * apiserver：集群统一入口，以restful方式，交给etcd存储
  * scheduler：节点调度，选择node节点应用部署
  * controller-manager：处理集群中常规后台任务，一个资源对应一个控制器
  * etcd：存储系统，用于保存集群相关的数据
* node组件
  * kubelet：master派到node节点代表，管理本机容器
  * kube-proxy：提供网络代理，负载均衡等操作

## 3、k8s核心概念

* Pod
  * 最小部署单元
  * 一组容器的集合
  * 共享网络
  * 生命周期是短暂的
* Controller
  * 确保预期的pod副本数量
  * 无状态应用部署
  * 有状态应用部署
  * 确保所有的node运行同一个pod
  * 一次性任务和定时任务
* Service
  * 定义一组pod的访问规则

# 第二部分 搭建K8S集群

## 1、搭建k8s环境平台规划

* 单master集群
* 多master集群

## 2、服务器硬件配置要求

* 测试环境
  * master：2核，4G，20G
  * node: 4核，8G，40G
* 生产环境

## 3、搭建k8s集群部署方式

* kubeadm
* 二进制包

# 第三部分 K8S核心技术

##  1 、K8S集群命令行工具kubectl 

命令语法格式：`$kubectl [command] [TYPE] [NAME] [flags]`

> command: `create`,  `get`,  `describe`, `delete` 
>
> TYPE: 可以为单数、复数和缩写的形式， 例如：`kubectl get pod1`,  `kubectl get pods pod1`,  `kubectl get po pod1` 

## 2、如何快速编写yaml文件

使用`kubectl create`命令快速生成yaml文件

```shell
kubectl create deployment web --image=nginx -o yaml --dry-run >my1.yaml
```

使用`kubectl get`命令导出yaml文件

```shell
kubectl get deploy nginx -o=yaml --export > my2.yaml
```

## 3、K8S核心技术Pod

### 3.1 Pod基本概念

1. 最小部署的单元
2. 包含多个容器（一组容器的集合）
3. 一个Pod中的容器共享网络命名空间
4. Pod是短暂的

### 3.2 Pod存在的意义

1. 创建容器使用docker，一个docker对应是一个容器，一个容器有进程，一个容器运行一个应用程序
2. Pod是多进行设计，运行多个应用程序，一个Pod有多个容器，一个容器里面运行一个应用程序
3. Pod存在为了亲密性应用
   * 两个应用之间进行交互
   * 网络之间调用
   * 两个应用需要频繁调用

### 3.3 Pod实现机制

1. 共享网络
   * 通过Pause容器，把其他业务容器加入到Pause容器里面，让所有业务容器在同一个名称空间中，可以实现网络共享。
2. 共享存储
   * 引入数据卷概念Volume，使用数据卷进行持久化存储。

### 3.4 镜像拉取策略

1. `IfNotPresent`: 默认值，镜像在宿主机上不存在时才拉取 
2. `Always`: 每次创建Pod都会重新拉取一次镜像
3. `Never`:  Pod永远不会主动拉取这个镜像

### 3.5 Pod资源限制

```yaml
spec.containers[].resources.limits.cpu
spec.containers[].resources.limits.memory
spec.containers[].resources.requests.cpu
spec.containers[].resources.requests.memory
```

### 3.6 Pod重启机制

1. `Always`:  当容器终止退出后，总是重启容器，默认策略
2. `OnFailure`: 当容器异常退出（退出状态码非0）时，才重启容器
3. `Never`: 当容器终止退出，从不重启容器

### 3.7 Pod健康检查

容器检查

**livenessProbe（存活检查）**：如果检查失败，将杀死容器，根据Pod的restartPolicy来操作

**readinessProbe（就绪检查）**：如果检查失败，Kubernetes会把Pod从service endpoints中剔除

> Probe支持以下三种检查方法：
>
> **httpGet**：发送HTTP请求，返回200-400范围状态码为成功
>
> **exec**：执行Shell命令返回状态码是0为成功
>
> **tcpSocket**：发起TCP Socket建立成功

### 3.8 调度策略

**master节点**

createpod -- apiserver -- etcd

scheduler -- apiserver -- etcd -- 调度算法，把pod调度到某个node节点上

**node节点**

kubelet -- apiserver -- 读取etcd拿到分配给当前节点pod -- docker创建容器

**影响调用的属性**

* Pod资源限制对Pod调用产生影响
* 节点选择器标签影响Pod调度
* 节点亲和性影响Pod调度
* 污点和污点容忍

## 4、K8S核心技术Controller

### 4.1 什么是controller

在集群上管理和运行容器的对象

### 4.2 Pod和controller关系

Pod是通过Controller实现应用的运维，比如伸缩，滚动升级等等。

Pod和Controller之间通过label标签建立关系，selector。

### 4.3 Deployment控制器应用场景

部署无状态应用

管理Pod和ReplicaSet

部署，滚动升级等功能

> 应用场景：web服务，微服务

### 4.4 yaml文件字段说明

```shell
kubectl create deployment web --image=nginx --dry-run -o yaml > web.yaml
kubectl apply -f web.yaml
kubectl expose deployment web -port=80 --type=NodePort --target-port=80 --name=web1 -o yaml > web1.yaml
```

### 4.5 Deployment控制器部署应用

### 4.6 升级回滚

```shell
[root@master ~]# kubectl get pods,svc
NAME                         READY   STATUS    RESTARTS   AGE
pod/nginx-6799fc88d8-msrlx   1/1     Running   3          3d14h

NAME                 TYPE        CLUSTER-IP    EXTERNAL-IP   PORT(S)        AGE
service/kubernetes   ClusterIP   10.1.0.1      <none>        443/TCP        3d14h
service/nginx        NodePort    10.1.59.115   <none>        80:30767/TCP   3d14h

# 应用升级
[root@master ~]# kubectl set image deployment nginx nginx=nginx:1.18
deployment.apps/nginx image updated
[root@master ~]# kubectl get pods
NAME                     READY   STATUS        RESTARTS   AGE
nginx-5bb97498bb-vz8bf   1/1     Running       0          16s
nginx-6799fc88d8-msrlx   1/1     Terminating   3          3d14h
[root@master ~]# kubectl get pods
NAME                     READY   STATUS    RESTARTS   AGE
nginx-5bb97498bb-vz8bf   1/1     Running   0          22s

# 查看升级状态
[root@master ~]# kubectl rollout status deployment nginx
deployment "nginx" successfully rolled out

[root@master ~]# kubectl rollout history deployment nginx
deployment.apps/nginx 
REVISION  CHANGE-CAUSE
1         <none>
2         <none>

# 回滚到上一个版本
[root@master ~]# kubectl rollout undo deployment nginx
deployment.apps/nginx rolled back
[root@master ~]# kubectl rollout history deployment nginx
deployment.apps/nginx 
REVISION  CHANGE-CAUSE
2         <none>
3         <none>

#回滚到指定版本
[root@master ~]# kubectl rollout undo deployment nginx --to-revision=2
deployment.apps/nginx rolled back
[root@master ~]# kubectl rollout history deployment nginx
deployment.apps/nginx 
REVISION  CHANGE-CAUSE
3         <none>
4         <none>

```

### 4.7 弹性伸缩

```shell
[root@master ~]# kubectl scale deployment nginx --replicas=10
deployment.apps/nginx scaled
[root@master ~]# kubectl get pods
NAME                     READY   STATUS              RESTARTS   AGE
nginx-5bb97498bb-6vqrt   0/1     ContainerCreating   0          7s
nginx-5bb97498bb-8ml5t   0/1     ContainerCreating   0          7s
nginx-5bb97498bb-9gdlf   1/1     Running             0          2m28s
nginx-5bb97498bb-bss54   0/1     ContainerCreating   0          7s
nginx-5bb97498bb-flgm7   0/1     ContainerCreating   0          7s
nginx-5bb97498bb-jctds   0/1     ContainerCreating   0          7s
nginx-5bb97498bb-wjnx6   0/1     ContainerCreating   0          7s
nginx-5bb97498bb-wlh5k   0/1     ContainerCreating   0          7s
nginx-5bb97498bb-xrclk   0/1     ContainerCreating   0          7s
nginx-5bb97498bb-xsmbh   0/1     ContainerCreating   0          7s
```

### 4.8 无状态和有状态

无状态：

* 认为Pod都是一样的
* 没有顺序要求
* 不用考虑在哪个node运行
* 随意进行伸缩和扩展

有状态：

* 上面因素都需要考虑到
* 让每个Pod独立的，保持Pod启动顺序和唯一性，唯一的网络标识符，持久存储，有序，比如mysql主从

### 4.9 部署有状态应用

无头Service：CluseterIP：none

StatefulSet部署有状态应用

```yaml
apiVersion: v1
kind: Service
metadata:
  name: nginx
  labels:
    app: nginx
spec: 
  ports: 
  - port: 80
    name: web
  cluseterIP: None
  selector:
    app: nginx
    
---

apiVersion: apps/v1
kind: StatefulSet
metadata:
  name: nginx-statefulset
  namespace: default
spec:
  serviceName: nginx
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
        image: nginx:latest
        ports:
        _ containerPort: 80
      
```

### 4.10 部署守护进程DaemonSet

在每个node上运行一个pod，新加入的node也同样运行在一个pod里面

> 例子：在每个node节点安装数据采集工具

### 4.11 job（一次性任务）

```yaml
apiVersion: batch/v1
kind: Job
metadata:
```

### 4.12 cronjob（定时任务）

```yaml
apiVersion: batch/v1beta1
kind: CronJob
metadata:
  name: hello
spec:
  schedule: "*/1 * * * *"
  jobTemplate: 
    spec:  
      template: 
        spec: 
          containers: 
            -name: hello
              image: busybox
              args: 
              - /bin/sh
              - -c
              - data; echo Hello from the Kubernetes cluster
            restartPolicy: OnFailure
```



## 5、K8S核心概念Service

### 5.1 Service存在意义

防止Pod失联（发现服务）

定义一组Pod访问策略（负载均衡）， vip虚拟ip

### 5.2 Pod和Service关系

根据label和selector标签建立关联的

### 5.3 常用Service类型

ClusterIP：集群的内部进行使用

NodePort：对外访问应用使用

LoadBalancer：对外访问应用使用，公有云 

```shell
[root@master ~]# kubectl expose deployment nginx --port=80 --target-port=80 --dry-run -o yaml > service.yam1
```

service.yaml

```yaml
apiVersion: v1
kind: Service
metadata:
  creationTimestamp: null
  labels:
    app: nginx
  name: nginx
spec:
  ports:
  - port: 80
    protocol: TCP
    targetPort: 80
  selector:
    app: nginx
  type: ClusterIP
status:
  loadBalancer: {}
```

## 6、K8S核心概念Secret

### 6.1 Secret

作用：加密数据存在etcd里面，让Pod容器以挂在Volume方式进行访问

> 场景：凭证

创建secret加密数据

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: mysecret
type: Opaque
data:
  username: YWRtaW4=
  password: MWYyZDFlMmU2N2Rm
```

以变量形式挂在到Pod容器中

```yaml
- name: SECRET_USERNAME
  valueFrom:
    secretKeyRef:
      name: mysecret
      key: username
- name: SECRET_PASSWORD
  valueFrom:
    secretKeyRef:
      name: mysecret
      key: password
```

以Volume形式挂载到Pod容器中

```yaml
apiVersion: v1
kind：Pod
metadata:
  name: mypod
spec:
  containers:
  - name: nginx
    image: nginx
    volumeMounts:
    - name: foo
      mountPath: "/etc/foo"
      readOnly: true
  volumes:
  - name: foo
    secret:
      secretName: mysecret
```

## 7、K8S核心技术Helm

### 7.1 helm引入

之前方式部署应用基本过程：

* 编写yaml文件：deployment，Service，Ingress

> 如果使用之前方式部署单一应用，少数服务的应用，比较合适；比如部署微服务项目，可能有几十个服务，每个服务都有一套yaml文件，需要维护大量yaml文件，版本管理特别不方便。

使用helm可以把这些yaml作为一个整体管理

实现yaml高效复用

使用helm应用级别的版本管理

### 7.2 helm介绍

Helm是一个K8S的包管理工具，就像Linux下的包管理工具，如yum/apt等，可以很方便的将之前打包好的yaml文件部署到K8S上

### 7.3 Helm三个重要概念

**helm**：一个命令行客户端工具

**Chart**:  把yaml打包，是yaml集合

**Release**：基于chart部署实体，应用级别的版本管理

### 7.4 Helm V3版本变化

V3版本删除Tiller

release可以在不同命名空间重用

将Chart推送到docker仓库中

### 7.5 Helm安装

```shell
# 解压helm压缩文件
[root@master ~]# tar zxvf helm-v3.5.4-linux-amd64.tar.gz 
linux-amd64/
linux-amd64/helm
linux-amd64/LICENSE
linux-amd64/README.md

# 把解压之后helm目录复制到/usr/bin目录下
[root@master ~]# mv linux-amd64/helm /usr/bin
```

### 7.6配置Helm仓库

添加仓库：

```shell 
[root@master ~]# helm repo add stable http://mirror.azure.cn/kubernetes/charts
"stable" has been added to your repositories

[root@master ~]# helm repo add aliyun https://kubernetes.oss-cn-hangzhou.aliyuncs.com/charts
"aliyun" has been added to your repositories

[root@master ~]# helm repo list
NAME  	URL                                                   
stable	http://mirror.azure.cn/kubernetes/charts              
aliyun	https://kubernetes.oss-cn-hangzhou.aliyuncs.com/charts

[root@master ~]# helm repo list
NAME  	URL                                                   
stable	http://mirror.azure.cn/kubernetes/charts              
aliyun	https://kubernetes.oss-cn-hangzhou.aliyuncs.com/charts
[root@master ~]# helm repo update
Hang tight while we grab the latest from your chart repositories...
...Successfully got an update from the "aliyun" chart repository
...Successfully got an update from the "stable" chart repository
Update Complete. ⎈Happy Helming!⎈
```

### 7.7 使用helm快速部署应用

第一步 使用命令搜索应用

`helm seatch repo 名称 (weave)`

第二部 根据搜索内容选择安装

`helm install 安装之后名称 搜索之后应用名称 `

查看安装之后状态

`helm list`

`helm status` 安装之后名称

更改服务配置

`kubectl edit svc ui-weave-scope`

### 7.8如何自己创建Chart

**使用命令创建chart**

```shell
[root@master ~]# helm create mychart
Creating mychart
[root@master ~]# cd mychart/
[root@master mychart]# ls
charts  Chart.yaml  templates  values.yaml
```

> **chart.yaml**: 当前chart属性配置信息
>
> **templates**: 编写yaml文件放到这个目录中
>
> **values.yaml**: yaml文件可以使用全局变量

**在templates文件夹创建两个yaml文件**

* deployment.yaml
* service.yaml

**安装mychart**

```shell
[root@master ~]# helm install nginx mychart/
NAME: nginx
LAST DEPLOYED: Sat May 22 19:33:02 2021
NAMESPACE: default
STATUS: deployed
REVISION: 1
TEST SUITE: None
```

**应用升级**

```shell
[root@master ~]# helm upgrade nginx mychart/
Release "nginx" has been upgraded. Happy Helming!
NAME: nginx
LAST DEPLOYED: Sat May 22 19:35:08 2021
NAMESPACE: default
STATUS: deployed
REVISION: 2
TEST SUITE: None
```

### 7.9 实现yaml高效复用

通过传递参数，动态渲染模板，yaml内容动态传入参数生成。

在`chart`有`values.yaml`文件，定义yaml文件全局变量。

* 在`values.yaml`定义变量和值

* 在具体yaml文件获取定义变量值

传值方式

`{{ .Values.name }}`

`{{ .Relsease.name }}`

## 8、K8S核心技术持久化存储

### 8.1 

数据卷 `emptydir`，是本地存储， pod重启，数据不存在了， 需要对数据持久化存储

nfs网络存储：pod重启，数据还存在的

第一步 找一台服务器nfs服务端，

安装nfs：`yum install -y nfs-utils`

设置挂载路径

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-dep1
spec:
  replicas: 1
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: ngnix
        image: nginx
        volumeMounts:
        - name: wwwroot
          mountPath: /usr/share/nginx/html
        ports:
        - containerPort: 80
      volumes:
        - name: wwwroot
          nfs:
            server: 192.168.10.136
            path: /data/nfs
```



```shell
[root@master pv]# kubectl describe pod nginx-dep1-77bb6f86d8-5hvr6

[root@master pv]# kubectl exec -it nginx-dep1-77bb6f86d8-5hvr6 bash
```

PV和PVC

PV：持久化存储，对存储资源进行抽象，对外提供可以调用的地方（生产者）

PVC: 用于调用，不需要关系内部实现细节（消费者）

实现流程

# 一、docker安装

```shell
# 安装最新版本
$ sudo apt-get update

$ sudo apt-get install \
apt-transport-https \
ca-certificates \
curl \
gnupg-agent \
software-properties-common
 
$ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
 
$ sudo apt-key fingerprint 0EBFCD88
 
$ sudo add-apt-repository \
"deb [arch=amd64] https://download.docker.com/linux/ubuntu \
$(lsb_release -cs) \
stable"
# 获取docker的repo
 
$ sudo apt-get update
 
 
$ sudo apt-get install docker-ce docker-ce-cli containerd.io
# 直接安装是安装最新版本的，这里需要安装指定版本，我们跳过
```

