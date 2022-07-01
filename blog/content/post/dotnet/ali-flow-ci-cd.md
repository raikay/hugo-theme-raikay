+++
author = "Raikay"  
title = "阿里云 流水线 云效Flow 实现持续集成、持续部署（CI/CD）"  
date = "2021-09-22"  
description = "阿里云 流水线 云效Flow 实现持续集成、持续部署（CI/CD）,dotnet core项目实现简单的自动化部署,通过HTTP WEB请求调用执行shell sh脚本实现简单的自动化部署 CI/CD"  
tags = [  
    "dotnet", "DevOps","Linux","Docker", "阿里云",
]  

+++

![IMG](http://blogimg.raikay.com/330618484553814016.png)

### 新建流水线
选择对应语言，新建流水线  
![IMG](http://blogimg.raikay.com/330611158820196352.png)

### 选择代码源

这个代码我托管在云效自带的Codeup

![IMG](http://blogimg.raikay.com/330611178130771968.png)

**代码结构**
![IMG](http://blogimg.raikay.com/330618637348114432.png)

**设置代码源触发**

开启代码源触发，并添加Webhook,Codeup会带动添加，如果是码云、github等其他托管平台，需要手动将下面webhook地址添加到代码托管平台对应的Webhook

![IMG](http://blogimg.raikay.com/330618666183954432.png)

### 配置构建

![IMG](http://blogimg.raikay.com/330626803687362560.png)

### 配置部署

新建主机，部署项目的服务器，如果是集群可以添加多台

![IMG](http://blogimg.raikay.com/330626732455497728.png)

如果账号内有阿里云ECS直接选择即可，否则选自有服务器，复制脚本在目标服务器执行

![IMG](http://blogimg.raikay.com/330618763575693312.png)

添加部署脚本和变量

![IMG](http://blogimg.raikay.com/330618817157926912.png)

**脚本**

`$image`变量是配置的镜像仓库地址，在上游构建时，会将构建好的docker镜像推送到这个地址

```
echo $image
#停止正在运行的容器
docker stop apidemo
#删除容器
docker rm apidemo
#拉取容器
docker pull $image
#运行新容器
docker run -d -p 1080:80 --name  apidemo $image
```

### 完成自动构建、部署

提交代码触发流水线

![IMG](http://blogimg.raikay.com/330618868710117376.png)

![IMG](http://blogimg.raikay.com/330618897248161792.png)


> 云效2020文档地址：https://help.aliyun.com/product/150040.html  
> 云效Flow地址：https://flow.aliyun.com/  
> 云效地址：https://devops.aliyun.com/  

### 相关文章：
[Docker环境安装及基础命令使用](https://blog.raikay.com/post/2020/docker/)  
[.Net Core项目使用Docker容器部署到Linux服务器](https://blog.raikay.com/post/2020/dotnet-docker/)  
[Linux系统Centos7部署DotNet Core项目及环境安装 ](https://blog.raikay.com/post/2019/dotnet-publish/)   
[dotnet项目执行shell脚本实现简单的自动化部署](https://blog.raikay.com/post/dotnet/easy-ci-cd/)  
[jenkins实现dotnet项目持续集成、持续部署（CI/CD）](https://blog.raikay.com/post/dotnet/jenkins/)  
[阿里云容器镜像服务提交代码自动构建Docker镜像](https://blog.raikay.com/post/2020/dotnet-core-aliyun/)     
[阿里云 流水线 云效Flow 实现持续集成、持续部署（CI/CD）](https://blog.raikay.com/post/dotnet/ali-flow-ci-cd/)  