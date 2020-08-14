+++
author = "Raikay"
title = "Linux服务器使用Docker部署donet core项目 "
date = "2020-02-11"
description = "Linux服务器使用Docker部署donet core 项目 应用程序 使用文档说明  简单教程  "
tags = [
    "dotnet",
    "docker",
]

+++

> 必备环境：Docker  
> [Docker 安装文档](/post/2020/docker/) : [http://blog.raikay.com/post/2020/docker/](/post/2020/docker/)  

### 创建项目

选择 ASP.NET Core Web 应用程序

![IMG](https://gitee.com/imgrep001/m1/raw/master/20200814171232.png)

可以在创建时直接勾选【启用Docker支持】选择Linux(图1)，也可以在已有项目右键添加Docker支持（图2）

图1

![IMG](https://gitee.com/imgrep001/m1/raw/master/20200814171413.png)

图2

![IMG](https://gitee.com/imgrep001/m1/raw/master/20200814171005.png)



项目发送至Linux服务器，我这里是上传到github,然后在linux服务器通过github下载项目

![IMG](https://gitee.com/imgrep001/m1/raw/master/20200814172015.png)



### git下载项目

```
git clone https://github.com/raikay/firstdemo.git
```

### 构建镜像

```
 docker build -t firstdemo . -f firstdemo/Dockerfile
```

![IMG](https://gitee.com/imgrep001/m1/raw/master/20200814172344.png)



如果构建dotnet环境太慢，可以提前下载加速镜像

```
docker pull ccr.ccs.tencentyun.com/dotnet-core/aspnet:3.1-buster-slim

docker tag ccr.ccs.tencentyun.com/dotnet-core/aspnet:3.1-buster-slim mcr.microsoft.com/dotnet/core/aspnet:3.1-buster-slim
```

```
docker pull ccr.ccs.tencentyun.com/dotnet-core/sdk:3.1-buster

docker tag ccr.ccs.tencentyun.com/dotnet-core/sdk:3.1-buster mcr.microsoft.com/dotnet/core/sdk:3.1-buster
```

### 运行镜像

```
docker run -d -p 1080:80 --name myfirstdemo  firstdemo
```

### 浏览器访问

```
http://192.168.198.131:1080/WeatherForecast
```

![IMG](https://gitee.com/imgrep001/m1/raw/master/20200814173048.png)

浏览器显示效果

![IMG](https://gitee.com/imgrep001/m1/raw/master/20200814172900.png)



