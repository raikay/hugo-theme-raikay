+++
author = "Raikay"
title = "Docker环境安装以及基础使用命令"
date = "2020-04-03"
description = "Docker 部署 环境  安装 文档 教程 镜像 分布式 "
tags = [
    "Docker",
]

+++



# 安装Docker

### yum加速：如果不是国内源可以替换加速

```
wget -O /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-7.repo #下载阿里yum源2 
yum makecache  #生成仓库缓存 
```

### 安装：

```
yum install docker -y
```



### 启动

```
systemctl start docker  #启动docker
systemctl enable docker #开机启动docker
```

### 查看	

```
systemctl status docker #查看docker状态
docker info  #查看详细信息
docker version #查看版本
```



### 加速

创建或修改 /etc/docker/daemon.json 文件

修改之后重启docker服务

```
{
  "registry-mirrors": [
    "https://registry.docker-cn.com",
    "http://hub-mirror.c.163.com",
    "https://docker.mirrors.ustc.edu.cn"
  ]
}
```
**加速源**

|  名称|  地址|
| -------------------- | ------------------------------ |
| 网易                 | http://hub-mirror.c.163.com|
| ustc                 |  https://docker.mirrors.ustc.edu.cn |
| 中国科技大学         |   https://docker.mirrors.ustc.edu.cn |
| Docker中国区官方镜像         | https://registry.docker-cn.com      |



### 重启
```
systemctl restart docker
```

加载

```
systemctl daemon-reload
```


# 使用Docker

### 查看已有镜像列表

```
docker images #查看有哪些镜像
```

### 搜索Nginx镜像

```
docker search nginx #就找第一个，下载最多的，官方镜像
```

### 下载镜像

```
docker pull nginx  #下载nginx镜像
```

### 启动镜像

```
docker run -p 8000:80 --name mynginx -d nginx 
#-p指定服务器8000端口,映射容器80 web端口，容器名为mynginx -d 守护进程模式启动（因为容器必须有进程在运行，否则结束就挂）
```

### 访问站点

```
 http://localhost:8000/
```

### 查看

```
docker ps #查看目前工作的容器
docker ps -a #查看所有运行过的容器
```

### 进出容器

```
#进入
docker exec -it 容器ID /bin/bash

#退出
exit
```

### 关闭镜像

```
docker stop nginx
```

### 删除镜像

```
#删除所有未使用的镜像
docker image prune 

#删除指定id的镜像
docker rmi <image id>
```

安装redis
```
docker pull redis:latest
docker run -itd --name redis-test -p 6379:6379 redis
docker exec -it redis-test /bin/bash
```

