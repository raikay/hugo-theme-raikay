+++
author = "Raikay"
title = ".Net Core项目使用Docker容器部署到Linux服务器Centos7"
date = "2020-02-11"
description = ".net core项目生成Docker容器部署到Linux服务器Centos7,应用程序,使用文档说明,简单教程"
tags = ["DotNet","Docker","Centos","Linux"]

+++

> 必备环境：Docker  
> [Docker 安装文档](/post/2020/docker/) : [http://blog.raikay.com/post/2020/docker/](/post/2020/docker/)  

### 创建项目

选择 ASP.NET Core Web 应用程序

![IMG](https://raikay.coding.net/p/code/d/m1/git/raw/master/20200814171232.png)

可以在创建时直接勾选【启用Docker支持】选择Linux(图1)，也可以在已有项目右键添加Docker支持（图2）

图1

![IMG](https://raikay.coding.net/p/code/d/m1/git/raw/master/20200814171413.png)

图2

![IMG](https://raikay.coding.net/p/code/d/m1/git/raw/master/20200814171005.png)



项目发送至Linux服务器，我这里是上传到github,然后在linux服务器通过github下载项目

![IMG](https://raikay.coding.net/p/code/d/m1/git/raw/master/20200814172015.png)



### git下载项目

```
git clone https://github.com/raikay/firstdemo.git
```

### 构建镜像

```
 docker build -t firstdemo . -f firstdemo/Dockerfile
```

![IMG](https://raikay.coding.net/p/code/d/m1/git/raw/master/20200814172344.png)



> 如果构建dotnet环境太慢，可以使用腾讯加速镜像下载
>
> ```
> docker pull ccr.ccs.tencentyun.com/dotnet-core/aspnet:3.1-buster-slim
>
> docker tag ccr.ccs.tencentyun.com/dotnet-core/aspnet:3.1-buster-slim mcr.microsoft.com/dotnet/core/aspnet:3.1-buster-slim
> ```
> 
> ```
> docker pull ccr.ccs.tencentyun.com/dotnet-core/sdk:3.1-buster
> 
> docker tag ccr.ccs.tencentyun.com/dotnet-core/sdk:3.1-buster mcr.microsoft.com/dotnet/core/sdk:3.1-buster
> ```
> 
> ```
> docker build -t firstdemo . -f firstdemo/Dockerfile
> ```

### 运行镜像

```
docker run -d -p 1080:80 --name myfirstdemo  firstdemo
```

### 浏览器访问

```
http://192.168.198.131:1080/WeatherForecast
```

![IMG](https://raikay.coding.net/p/code/d/m1/git/raw/master/20200814173048.png)

浏览器显示效果

![IMG](https://raikay.coding.net/p/code/d/m1/git/raw/master/20200814172900.png)

### Dockerfile解释

```sh
#使用asp.net core 3.1作为基础镜像，起一个别名为base
FROM mcr.microsoft.com/dotnet/core/aspnet:3.1-buster-slim AS base
#设置容器的工作目录为/app
WORKDIR /app
#暴露80端口
EXPOSE 80

#使用.net core sdk 3.1作为基础镜像，起一个别名为build
FROM mcr.microsoft.com/dotnet/core/sdk:3.1-buster AS build
#设置容器的工作目录为/src
WORKDIR /src
#拷贝WebApplication1/WebApplication1.csproj项目文件到容器中的/src/WebApplication1/目录
COPY ["WebApplication1/WebApplication1.csproj", "WebApplication1/"]
#执行dotnet restore命令，相当于平时用vs还原nuget包
RUN dotnet restore "WebApplication1/WebApplication1.csproj"
#拷贝当前目录的文件到到容器的/src目录
COPY . .
#设置容器的工作目录为/src/WebApplication1
WORKDIR "/src/WebApplication1"
#执行dotnet build命令，相当于平时用vs生成项目。以Release模式生成到容器的/app/build目录
RUN dotnet build "WebApplication1.csproj" -c Release -o /app/build

#将上面的build(.net core sdk 3.1)作为基础镜像，又重命名为publish
FROM build AS publish
#执行dotnet publish命令，相当于平时用vs发布项目。以Release模式发布到容器的/app/publish目录
RUN dotnet publish "WebApplication1.csproj" -c Release -o /app/publish

#将上面的base(asp.net core 3.1)作为基础镜像，又重命名为final
FROM base AS final
#设置容器的工作目录为/app
WORKDIR /app
#拷贝/app/publish目录到当前工作目录
COPY --from=publish /app/publish .
#指定容器入口命令，容器启动时会运行dotnet WebApplication1.dll
ENTRYPOINT ["dotnet", "WebApplication1.dll"]
```

