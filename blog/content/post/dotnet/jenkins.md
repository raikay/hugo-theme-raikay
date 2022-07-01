+++
author = "Raikay"  
title = "jenkins实现dotnet项目持续集成、持续部署（CI/CD）"  
date = "2021-09-21"  
description = "dotnet+jenkins实现自动化部署，分布式Jenkins几点添加，持续交付，添加宿主机节点，简单 自动化部署 CI/CD"  
tags = [  
    "dotnet", "CentOS","Linux","jenkins","Docker", "DevOps", 
]  

+++



## 添加节点
 这个jenkins是用docker安装的，我们要操作宿主机构建项目，不是这个容器，所以要把这个容器本身的节点停止，添加宿主机节点  
![IMG](http://blogimg.raikay.com/330627946962358272.png)

### 断开jenkins容器节点 Master 

> 也可以使用标签限制

![IMG](http://blogimg.raikay.com/330627974233722880.png)

### 新建节点
![IMG](http://blogimg.raikay.com/330627997226897408.png)

### 添加宿主机连接信息
![IMG](http://blogimg.raikay.com/330628025358094336.png)

### 添加宿主机认证用户
![IMG](http://blogimg.raikay.com/330628070421696512.png)


### 生成ssh公钥和私钥

执行：`ssh-keygen` 一路回车

![IMG](http://blogimg.raikay.com/330628098401898496.png)



进入.ssh文件夹：  

```
cd .ssh/
```

`id_rsa`：私钥  
`id_rsa.pub`：公钥  

把公钥写到`authorized_keys`文件：  
```
cat id_rsa.pub > authorized_keys
```

复制私钥到jenkins  

```
cat id_rsa
```

![IMG](http://blogimg.raikay.com/330628518121705472.png)

![IMG](http://blogimg.raikay.com/330628546835910656.png)

## 添加触发器
`token`可以区分项目  
![IMG](http://blogimg.raikay.com/330628631640543232.png)

![IMG](http://blogimg.raikay.com/330628581128540160.png)

把地址复制到代码托管平台，设置触发

![IMG](http://blogimg.raikay.com/330628661009059840.png)

## 添加构建脚本

![IMG](http://blogimg.raikay.com/330628678046322688.png)

Jenkins构建完成会自动关闭进程及其子进程，造成`nohup` 无效，需要在命令内加参数`BUILD_ID=DONTKILLME`

`publish.sh`：

```shell
#!/bin/bash
#如果8081被占用杀掉进程
kill -9 $(lsof -i:8081 -t)
cd /home/web/web-demo/
#拉代码
git pull
#发布代码到publish目录
dotnet publish -o publish
cd publish/
#后台启用项目，端口8081
nohup dotnet WebDemo.dll --urls http://0.0.0.0:8081 &
```

## 构建记录

![IMG](http://blogimg.raikay.com/330628175312850944.png)

### 相关文章：
[Docker环境安装及基础命令使用](https://blog.raikay.com/post/2020/docker/)  
[.Net Core项目使用Docker容器部署到Linux服务器](https://blog.raikay.com/post/2020/dotnet-docker/)  
[Linux系统Centos7部署DotNet Core项目及环境安装 ](https://blog.raikay.com/post/2019/dotnet-publish/)   
[dotnet项目执行shell脚本实现简单的自动化部署](https://blog.raikay.com/post/dotnet/easy-ci-cd/)  
[jenkins实现dotnet项目持续集成、持续部署（CI/CD）](https://blog.raikay.com/post/dotnet/jenkins/)  
[阿里云容器镜像服务提交代码自动构建Docker镜像](https://blog.raikay.com/post/2020/dotnet-core-aliyun/)  