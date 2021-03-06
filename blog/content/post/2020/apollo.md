+++
author = "Raikay"
title = "使用Docker部署Apollo多个环境在一台(Linux)服务器"
date = "2020-04-05"
description = "从一个环境版本安装开始，包含一个环境的安装方法，Docker镜像部署Apollo在linux系统单机单环境一个环境和单机多环境在一台机器上 安装 文档 教程 镜像 分布式 环境 免费"
tags = [
    "apollo",
    "docker",
]

weight=98

+++

# 一、一个环境的安装
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



![IMG](https://gitee.com/imgrep001/m1/raw/master/20200811145031.png)

如果不能访问，检查一下防火墙是否通行。  



默认带了一个SampleApp项目

![](https://gitee.com/imgrep001/m1/raw/master/20200811145145.png)

添加配置：

![IMG](https://gitee.com/imgrep001/m1/raw/master/20200811145356.png)

发布后生效

![IMG](https://gitee.com/imgrep001/m1/raw/master/20200811145535.png)



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

![IMG](https://gitee.com/imgrep001/m1/raw/master/20200811151609.png)



# 二、多个环境安装在一台机器

### 一、搭建MySql

**1、拉取官方镜像**
（我们这里选择5.7，如果不写后面的版本号则会自动拉取最新版）

```
docker pull mysql:5.7   # 拉取 mysql 5.7
docker pull mysql       # 拉取最新版mysql镜像
docker images           # 检查是否拉取成功
```

**2、运行镜像**

```
docker run -p 23306:3306 --name mysql -e MYSQL_ROOT_PASSWORD=123456 -d mysql:5.7
```

参数说明：

`–name`：容器名，此处命名为mysql  
`-e`：配置信息，此处配置mysql的root用户的登陆密码  
`-p`：端口映射，此处映射 主机23306端口 到 容器的3306端口  

**3、创建数据库**

根据`\apollo\scripts\sql`目录下的两个脚本 创建三个数据库  

ApolloConfigDBPro : `apolloconfigdb.sql`  
ApolloConfigDBDev  : `apolloconfigdb.sql`  
ApolloPortalDB : `apolloportaldb.sql`  

**4、修改值**

修改 `ApolloPortalDB`库中`ServerConfig`表`apollo.portal.envs`的值为`dev,pro`  
![IMG](https://gitee.com/imgrep001/m1/raw/master/20200811155302.png)



修改 `ApolloConfigDBPro`库中`ServerConfig`表eureka.service.url的值 为`http://localhost:8083/eureka/`

![IMG](https://gitee.com/imgrep001/m1/raw/master/20200811160145.png)


### 二、搭建Apollo

**拉取镜像**

```
docker pull idoop/docker-apollo
```

**运行容器**

注意 "=" 附近不能有空格

```
docker run --net="host" --name myapollo -d \
 -e PORTAL_DB='jdbc:mysql://192.168.198.128:23306/ApolloPortalDB?characterEncoding=utf8' \
 -e PORTAL_DB_USER='root' \
 -e PORTAL_DB_PWD='123456' \
 -e DEV_DB='jdbc:mysql://192.168.198.128:23306ApolloConfigDBDev?characterEncoding=utf8' \
 -e DEV_DB_USER='root' \
 -e DEV_DB_PWD='123456' \
 -e PRO_DB='jdbc:mysql://192.168.198.128:23306/ApolloConfigDBPro?characterEncoding=utf8' \
 -e PRO_DB_USER='root' \
 -e PRO_DB_PWD='123456' \
 idoop/docker-apollo:latest 
```



时间有点久，等三、五分钟后，访问 `http://192.168.198.128:8070/`

等待时可以 `docker ps` 查看状态

![IMG](https://gitee.com/imgrep001/m1/raw/master/20200811172434.png)



### 三、Apollo基本使用



**创建项目**

![IMG](https://gitee.com/imgrep001/m1/raw/master/20200811172640.png)



现在已经是两个环境 DEV、PRO

![IMG](https://gitee.com/imgrep001/m1/raw/master/20200811172833.png)



### 新增配置

默认只在DEV环境创建，也可以在【选择集群】勾选PRO，同时在两个环境创建。

![IMG](https://gitee.com/imgrep001/m1/raw/master/20200811173324.png)



也可以根据实际项目进度，部署生产环境时同步到PRO

图1

![IMG](https://gitee.com/imgrep001/m1/raw/master/20200811173713.png)

图2

![IMG](https://gitee.com/imgrep001/m1/raw/master/20200811173733.png)



# 问题解决

1、执行 `docker-compose up`  如果提示以下错误：

```
docker-compose: not found
```

安装pip

```
pip -v #检查是否安装

yum -y install epel-release
yum -y install python-pip
#升级
pip install --upgrade pip
```

安装docker-compose：

```
pip install docker-compose
#检查是是否成功
docker-compose -version
```

如果超时失败，可以临时使用国内源

```
pip install docker-compose -i https://mirrors.aliyun.com/pypi/simple/
```



2、 error: command 'gcc' failed with exit status 1  

是否忘记 升级pip  

3、环境科普  

DEV：Development environment，开发环境，用于开发者调试使用  

FAT：Feature Acceptance Test environment，功能验收测试环境，用于软件测试者测试使用  

UAT：User Acceptance Test environment，用户验收测试环境，用于生产环境下的软件测试者测试使用  

PRO：Production environment，生产环境  

4、继续扩展  

如果想添加 UAT、FAT等环境添加对应的数据库  

修改 `ApolloConfigDBUat`库中`ServerConfig`表eureka.service.url的值 为`http://localhost:8081/eureka/`  

修改 `ApolloConfigDBFat`库中`ServerConfig`表eureka.service.url的值 为`http://localhost:8082/eureka/ ` 

修改 `ApolloPortalDB`库中`ServerConfig`表`configView.memberOnly.envs`的值为`pro,uat,fat`    

同时环境变量增加对应的数据库连接、账号密码  

[使用指南](https://github.com/ctripcorp/apollo/wiki/Apollo%E4%BD%BF%E7%94%A8%E6%8C%87%E5%8D%97#%E4%B8%80%E6%99%AE%E9%80%9A%E5%BA%94%E7%94%A8%E6%8E%A5%E5%85%A5%E6%8C%87%E5%8D%97)

![IMG](https://gitee.com/imgrep001/m1/raw/master/20200812105011.png)

鸣谢：

>  https://www.cnblogs.com/756623607-zhang/p/12726276.html
> 
> https://www.cnblogs.com/sablier/p/11605606.html
> 
> https://www.cnblogs.com/sablier/p/11605606.html #Docker-MySql
> 
> https://github.com/ctripcorp/apollo
> 
> https://github.com/idoop/docker-apollo