+++
author = "Raikay"
title = "Docker环境安装及基础命令使用"
date = "2020-04-03"
description = "Docker 是一个开源的应用容器引擎，让开发者可以打包他们的应用以及依赖包到一个可移植的镜像中，然后发布到任何流行的 Linux或Windows操作系统的机器上，也可以实现虚拟化。容器是完全使用沙箱机制，相互之间不会有任何接口。Docker环境的安装以及基础命令使用，并且配置国内源加速"
tags = [
    "docker","环境安装","国内源"
]

+++

Docker 是一个开源的应用容器引擎，让开发者可以打包他们的应用以及依赖包到一个可移植的镜像中，然后发布到任何流行的 Linux或Windows操作系统的机器上，也可以实现虚拟化。容器是完全使用沙箱机制，相互之间不会有任何接口。

# 安装Docker

### 为yum配置加速

centos默认国外源，安装过程不是很理想

```shell
#下载阿里yum源2 
wget -O /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-7.repo 
yum clean all  #清理缓存
yum makecache  #生成仓库缓存 
```

### 安装：

1、安装必要依赖包

```shell
yum install -y yum-utils device-mapper-persistent-data lvm2
```
2、添加阿里镜像稳定版仓库

```shell
yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
```

3、安装docker-ce

```shell
yum install -y docker-ce docker-ce-cli containerd.io
```

### 配置docker加速

创建或修改 `daemon.json` 文件

修改之后重启docker服务

```shell
vim  /etc/docker/daemon.json
```


```json
{
  "registry-mirrors" : [
    "http://registry.docker-cn.com",
    "http://docker.mirrors.ustc.edu.cn",
    "http://hub-mirror.c.163.com"
  ],
  "insecure-registries" : [
    "registry.docker-cn.com",
    "docker.mirrors.ustc.edu.cn"
  ],
  "debug" : true,
  "experimental" : true
}
```

开机启动

```
systemctl enable docker #开机启动docker
```

### 重启

```shell
systemctl daemon-reload #加载
systemctl restart docker  #重启docker
```

### 查看	

```shell
systemctl status docker #查看docker状态
docker info  #查看详细信息
docker version #查看版本
```

# 使用Docker

查看已有镜像列表

```shell
docker images #查看有哪些镜像
```
查看

```sh
docker ps #查看目前工作的容器
docker ps -a #查看所有运行过的容器
docker ps -aq #查看所有容器id
```

搜索Nginx镜像

```shell
docker search nginx #就找第一个，下载最多的，官方镜像
```

下载镜像

```shell
docker pull nginx  #下载nginx镜像
```

启动镜像

```shell
docker run -p 8000:80 --name mynginx -d nginx 
#-p 指定服务器8000端口,映射容器80 web端口
#-name 容器名为mynginx 
#-d 守护进程模式启动（因为容器必须有进程在运行，否则结束就挂
```
| 参数   | 说明 |
| --------- | ------------------ |
| run   | 创建并运行一个容器 |
| -d    | 放入后台           |
| -p    | 端口映射           |
| -v | 挂载目录     |
| -env | 环境变量          |

启动后URL

```
http://localhost:8000/
```


进入容器

```sh
#进入
docker exec -it 容器ID /bin/bash

#退出
exit
```

关闭镜像

```sh
#停止Nginx容器
docker stop nginx #id或name

#停止所有容器
docker stop $(docker ps -aq)

#删除所有容器
docker rm $(docker ps -aq)
```

删除镜像

```sh
#删除所有未使用的镜像
docker image prune 

#删除指定id或name的镜像
docker rmi <image id>

# 删除所有镜像
docker rmi $(docker images -q)
```

安装redis
```sh
docker pull redis:latest
docker run -itd --name redis-test -p 6379:6379 redis
docker exec -it redis-test /bin/bash
```

容器保存为镜像
```
#docker commit 容器名 镜像名
docker commit mynginx mynginx_i
```

镜像输出为文件
```
docker save -o myNginxFileName.tar mynginx_i
```
恢复镜像

```
docker load -i myNginxFileName.tar
```

构建
```
docker build -t '镜像名称' .
```

源简写

```json
{
  "registry-mirrors": [
    "https://registry.docker-cn.com",
    "http://hub-mirror.c.163.com",
    "https://docker.mirrors.ustc.edu.cn"
  ]
}
```

卸载已安装的docker版本
```sh
yum -y remove docker*

#删除旧版本Docker文件
#不删除目录，就不会删除已安装的镜像及容器
sudo rm /var/lib/docker/ -rf
```


# 扩展
**问题解决**
添加阿里源时有时会报错，如果报错，使用如下命令使用官方源  
```shell
#删除异常源
rm -f /etc/yum.repos.d/docker-ce.repo
#使用官方源
yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
#清华大学源
#https://mirrors.tuna.tsinghua.edu.cn/docker-ce/linux/centos/docker-ce.repo
```
**加速源**

|  名称|  地址|
| -------------------- | ------------------------------ |
| 网易                 | `http://hub-mirror.c.163.com`|
| 阿里云(需登录)                 |  `https://<your_code>.mirror.aliyuncs.com` |
| 中国科技大学         |   `https://docker.mirrors.ustc.edu.cn` |
| Docker中国区官方镜像         | `https://registry.docker-cn.com`      |
| DaoCloud 镜像站         | `http://f1361db2.m.daocloud.io`      |
| Azure 中国镜像         | `https://dockerhub.azk8s.cn`      |
| 七牛云         |  `https://reg-mirror.qiniu.com`      |
| 腾讯云         |  `https://mirror.ccs.tencentyun.com`     |

### 安装 docker-compose

```shell
#如果没有pip，先安装pip
yum -y install epel-release
yum -y install python2-pip
#或yum -y install python3-pip

#升级
pip install --upgrade pip
#安装docker-compose
pip install docker-compose
#查看版本
docker-compose -version
```

### 第三方脚本：

```
#安装
curl -sSL https://get.docker.com/ | bash -x
#卸载 ubuntu 通过dpkg加grep 查看具体装的什么版本，然后卸载。centos通过rpm -qa，然后-e
apt-get purge lxc-docker
```

### 轻量级可视化管理工具`Portainer`

###  相关问题：

**1、docker stop 与 docker kill 均可以将容器停掉，但二者究竟有什么区别呢？**

docker stop，支持“优雅退出”。先发送SIGTERM信号，在一段时间之后（10s）再发送SIGKILL信号。Docker内部的应用程序可以接收SIGTERM信号，然后做一些“退出前工作”，比如保存状态、处理当前请求等。

docker kill， 发送SIGKILL信号，应用程序直接退出。

**2、为什么是docker-ce?**

>https://zhuanlan.zhihu.com/p/305572519
- docker-ce 社区版，免费
- docker-ee 商业版，收费


**鸣谢：**
> https://baike.baidu.com/item/Docker/13344470
> https://www.cnblogs.com/hellxz/p/11044012.html    
> https://www.jianshu.com/p/6eea18d6fb39 #镜像的备份      



