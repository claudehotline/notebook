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

