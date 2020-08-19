

+++
author = "Raikay"
title = "DotNet Core3.1环境安装及部署项目"
date = "2019-09-06"
description = "DotNet Core 环境安装及部署项目 net core 3.1"
tags = [
    "dotnet",
]

+++



安装 dotnet core

```sh
#使用微软官方的源
sudo rpm -Uvh https://packages.microsoft.com/config/rhel/7/packages-microsoft-prod.rpm

#更新软件源
sudo yum update

#可以先搜索看一下列表
#yum search dotnet

#安装
sudo yum install -y dotnet-sdk-3.1.x86_64

#查看是否安装成功
dotnet --version
```



下载项目

```sh
git clone https://github.com/raikay/firstdemo.git
```

生成项目

```sh
#进入生成目录
cd firstdemo/firstdemo
#生成
dotnet publish -c release -o "publish"
# -c 指定release; -o 指定生成目录
```

运行项目

```
dotnet firstdemo.dll --urls http://0.0.0.0:80
```

浏览器访问地址查看结果

```
http://192.168.198.131/WeatherForecast
```

![IMG](https://gitee.com/imgrep001/m1/raw/master/20200819155335.png)