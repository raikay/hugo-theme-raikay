+++
author = "Raikay"  
title = "使用Docker在CentOS系统安装Jenkins"  
date = "2021-09-20"  
description = "使用Docker在 CentOS 系统 安装 Jenkins 自动化 部署 CI/CD工具 ， docker安装Jenkins，DevOps，持续集成，持续部署"  
tags = [  
         "CentOS","Linux","jenkins","Docker","DevOps" ,
]  

+++
> 需要先安装Docker：   
> [Docker的安装以及使用 : https://blog.raikay.com/post/2020/docker/](https://blog.raikay.com/post/2020/docker/)

### 下载Jenkins镜像
```
docker pull jenkins/jenkins:lts #最新版
```

### 创建映射目录
容器映射此目录以持久化数据  
```
mkdir /data/jenkins_home/
```

### 修改用户组
docker容器中jenkins用户和用户组id为1000，需要修改后目录才能映射成功  
```
chown -R 1000:1000 /data/jenkins_home
```


### 启动Jenkins容器	 
```shell
docker run -d --name jenkins -p 80:8080 -v /data/jenkins_home:/var/jenkins_home jenkins/jenkins:lts;

#备注：
#-d 启动在后台
#--name 容器名字
#-p 端口映射（80：宿主主机端口，8080：容器内部端口）
#-v 数据卷挂载映射（/data/jenkins_home：宿主主机目录，另外一个即是容器目录）
#enkins/jenkins:lts Jenkins镜像（最新版）
```

### 访问Jenkins
地址解析的域名  
```diff
http://jenkins.raikay.com/
```
根据提示的路径找到初始密码  
```shell
#进入容器
docker exec -it jenkins bash
#查看密码
cat /var/jenkins_home/secrets/initialAdminPassword
```
![IMG](http://blogimg.raikay.com/330629592639475712.png)

### 安装推荐的插件

![IMG](http://blogimg.raikay.com/330629568849383424.png)

![IMG](http://blogimg.raikay.com/330629484237688832.png)

### 创建管理员

![IMG](http://blogimg.raikay.com/330629466713886720.png)

### 安装已完成

![IMG](http://blogimg.raikay.com/330629408324980736.png)



### 扩展

**设置时区**

方法一：在【系统管理】-【脚本命令行】里运行

```
System.setProperty('org.apache.commons.jelly.tags.fmt.timeZone', 'Asia/Shanghai')
```



![IMG](http://blogimg.raikay.com/330629376666374144.png)

![IMG](http://blogimg.raikay.com/330629349659250688.png)

方法二：

通过docker改容器时区

```
docker run ... -e JAVA_OPTS=-Duser.timezone=Asia/Shanghai
```



**添加windows节点**

![IMG](http://blogimg.raikay.com/330629299071750144.png)

保存后，返回节点，下载`agent.jar`到目标主机，并在目标主机执行这条命令

> jenkins容器的50000 端口需要映射到宿主机

![IMG](http://blogimg.raikay.com/330629278855204864.png)

**为 [构建触发器] 添加过滤**

读取webhook请求的json，gitee为例

`ref`  ： 分支

`fullname`  : 项目名

![IMG](http://blogimg.raikay.com/330629257392951296.png)

过滤规则

![IMG](http://blogimg.raikay.com/330629236396265472.png)

### 插件

http请求插件：HTTP Request

### 相关文章：

[Docker环境安装及基础命令使用](https://blog.raikay.com/post/2020/docker/)  
[.Net Core项目使用Docker容器部署到Linux服务器](https://blog.raikay.com/post/2020/dotnet-docker/)  
[Linux系统Centos7部署DotNet Core项目及环境安装 ](https://blog.raikay.com/post/2019/dotnet-publish/)   
[dotnet项目执行shell脚本实现简单的自动化部署](https://blog.raikay.com/post/dotnet/easy-ci-cd/)  
[jenkins实现dotnet项目持续集成、持续部署（CI/CD）](https://blog.raikay.com/post/dotnet/jenkins/)  
[阿里云容器镜像服务提交代码自动构建Docker镜像](https://blog.raikay.com/post/2020/dotnet-core-aliyun/)      