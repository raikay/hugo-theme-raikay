+++
author = "Raikay"
title = "Docker环境安装以及基础使用命令"
date = "2020-04-03"
description = "Docker 部署 环境  安装 文档 教程 镜像 分布式 "
tags = [
    "docker",
]

+++



# 安装Docker

### yum加速：如果不是国内源可以替换加速

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


> 添加阿里源时有时会报错，如果报错，使用如下命令使用官方源
> ```shell
> #删除异常源
> rm -f /etc/yum.repos.d/docker-ce.repo
> #使用官方源
> yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
> ```

3、安装Docker-CE

```shell
yum install -y docker-ce docker-ce-cli containerd.io
```

### 配置加速

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
    "http://hub-mirror.c.163.com",
    "http://ovfftd6p.mirror.aliyuncs.com"
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



**加速源**

|  名称|  地址|
| -------------------- | ------------------------------ |
| 网易                 | http://hub-mirror.c.163.com|
| ustc                 |  https://docker.mirrors.ustc.edu.cn |
| 中国科技大学         |   https://docker.mirrors.ustc.edu.cn |
| Docker中国区官方镜像         | https://registry.docker-cn.com      |



# 使用Docker

查看已有镜像列表

```shell
docker images #查看有哪些镜像
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
#-p指定服务器8000端口,映射容器80 web端口，容器名为mynginx -d 守护进程模式启动（因为容器必须有进程在运行，否则结束就挂
```

访问站点

```
 http://localhost:8000/
```

查看

```sh
docker ps #查看目前工作的容器
docker ps -a #查看所有运行过的容器
```

进出容器

```sh
#进入
docker exec -it 容器ID /bin/bash

#退出
exit
```

关闭镜像

```sh
docker stop nginx
```

删除镜像

```sh
#删除所有未使用的镜像
docker image prune 

#删除指定id的镜像
docker rmi <image id>
```

安装redis
```sh
docker pull redis:latest
docker run -itd --name redis-test -p 6379:6379 redis
docker exec -it redis-test /bin/bash
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

鸣谢：
https://www.cnblogs.com/hellxz/p/11044012.html

