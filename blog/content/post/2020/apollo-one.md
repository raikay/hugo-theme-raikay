+++
author = "Raikay"
title = "使用Docker部署Apollo一个环境docker-quick-start版"
date = "2020-04-05"
description = "从一个环境版本安装开始，包含一个环境的安装方法，Docker镜像部署Apollo在linux系统单机单环境一个环境和单机多环境在一台机器上 安装 文档 教程 镜像 分布式 环境 免费"
tags = [
    "apollo",
    "docker",
]

+++

> 必备环境：Docker  
> [Docker 安装](/post/2020/docker/) : [http://blog.raikay.com/post/2020/docker/](/post/2020/docker/)  

### 1、下载Apollo源码

```
git clone https://github.com/ctripcorp/apollo.git
```

### 2、进入docker-quick-start 目录

```
cd apollo/scripts/docker-quick-start
```



### 3、安装启动

```
docker-compose up
```

看到下面日志表示已经安装成功

```
apollo-quick-start    | Waiting for config service startup.....
apollo-quick-start    | Config service started. You may visit http://localhost:8080 for service status now!
apollo-quick-start    | Waiting for admin service startup.
apollo-quick-start    | Admin service started
apollo-quick-start    | ==== starting portal ====
apollo-quick-start    | Portal logging file is ./portal/apollo-portal.log
apollo-quick-start    | Started [237]
apollo-quick-start    | Waiting for portal startup.....
apollo-quick-start    | Portal started. You can visit http://localhost:8070 now!
```



### 4、基本使用

管理后台：  
地址：http://localhost:8070  

用户名密码：apollo/admin  

Mysql：  
localhost:13306，用户名是root，密码为空  
![IMG](https://raikay.coding.net/p/code/d/m1/git/raw/master/20200811145031.png)

如果不能访问，检查一下防火墙是否通行。  
默认带了一个SampleApp项目

![](https://raikay.coding.net/p/code/d/m1/git/raw/master/20200811145145.png)

添加配置：
![IMG](https://raikay.coding.net/p/code/d/m1/git/raw/master/20200811145356.png)

发布后生效
![IMG](https://raikay.coding.net/p/code/d/m1/git/raw/master/20200811145535.png)

查看日志
```
#进入容器
docker exec -it apollo-quick-start bash
#日志目录
/apollo-quick-start/service
/apollo-quick-start/portal
#查看
cat apollo-service_apollo-quick-startservice.log
```

停止服务
```
docker stop apollo-db
docker stop apollo-quick-start
```

启动客户端程序，在控制台获取value
```
docker exec -i apollo-quick-start /apollo-quick-start/demo.sh client
```
![IMG](https://raikay.coding.net/p/code/d/m1/git/raw/master/20200811151609.png)



相关文章：  
[使用Docker部署Apollo多个环境在一台(Linux)服务器](http://blog.raikay.com/post/2020/apollo/)
