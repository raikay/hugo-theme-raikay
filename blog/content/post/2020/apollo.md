+++
author = "Raikay"
title = "Docker部署Apollo单机单环境和单机多环境"
date = "2020-04-05"
description = "Docker部署Apollo单机单环境和单机多环境 安装 文档 教程 镜像 分布式 环境"
tags = [
    "Apollo",
]

+++

# 单环境安装及基本使用
> 必备环境：Docker  
> [Docker 安装](/post/2020/docker/) : [http://blog.raikay.com/post/2020/docker/](/post/2020/docker/)  

### 一、下载Apollo源码

```
git clone https://github.com/ctripcorp/apollo.git
```



### 二、进入docker-quick-start 目录

```
cd apollo/scripts/docker-quick-start
```



### 三、安装成功

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



### 四、基本使用

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



# 单机多环境安装及基本使用

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

默认只创建一个DEV环境，也可以在【选择集群】勾选PRO，同时创建两个环境。

![IMG](https://gitee.com/imgrep001/m1/raw/master/20200811173324.png)



也可以根据实际项目进度，部署生产环境事在同步到PRO

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



2、 error: command 'gcc' failed with exit status 1

是否忘记 升级pip





鸣谢：

https://www.cnblogs.com/756623607-zhang/p/12726276.html

https://www.cnblogs.com/sablier/p/11605606.html

https://www.cnblogs.com/sablier/p/11605606.html #Docker-MySql

https://github.com/ctripcorp/apollo

https://github.com/idoop/docker-apollo