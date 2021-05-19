# 第一部分

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

# 第二部分

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

