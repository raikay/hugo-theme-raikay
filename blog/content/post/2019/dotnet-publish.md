

+++
author = "Raikay"
title = "Linux系统Centos7部署DotNet Core项目及环境安装"
date = "2019-09-06"
description = "DotNet Core 环境安装及部署项目 net core 3.1，supervisor 进程管理，创建守护进程、程序持久运行 安装 部署 文档 教程"
tags = [
    "dotnet",
]

+++

# 1、环境安装及项目运行

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

至此已经成功运行！

# 2、supervisor创建守护进程

到上面的步骤程序运行没问题了，但是如果退出当前控制台，程序就会结束运行。  

我们可以使用supervisor托管程序，创建守护进程，使程序持续运行。  

安装supervisor  

```
yum install -y epel-release #安装扩展源
yum install -y supervisor
```

进入 supervisor控制台

```
supervisorctl
```

可以进入 supervisor 控制台，表示服务安装成功，并已成功启动

![IMG](https://gitee.com/imgrep001/m1/raw/master/20200819170821.png)

现在 可以ctrl+c退出。

开机启动

```
systemctl enable supervisord
```

创建应用程序配置文件

```
#进入supervisor配置文件目录
cd /etc/supervisord.d
编辑创建文件
vim demo.ini
```

文件内容：

```sh
[program:demo]
#输入执行命令，这里表示 dotnet  firstdemo.dll
command=/usr/bin/dotnet  firstdemo.dll --urls http://0.0.0.0:80;
#应用程序根目录 
directory=/root/firstdemo/firstdemo/publish ;
#是否自动启动，当 supervisor 加载该配置文件的时候立即启动它 
autostart=true ;
#是否自动重启，当执行 dotnet  firstdemo.dll 启动失败时，会重复的自动重启 
autorestart=true ;
#该配置文件输出单个日志文件的大小 
logfile_maxbytes=50MB ;
#日志备份个数 
logfile_backups=10 ;
#记录日志级别 
loglevel=info ;
#指定标准错误输出日志文件 
stderr_logfile=/data/logs/demo/demo.err.log ;
#指定标准输出日志文件 
stdout_logfile=/data/logs/demo/demo.out.log ;
#可配置环境变量，该环境变量将通过执行 dotnet  firstdemo.dll 命令的时候传入到 .NET Core 应用程序中  
environment=ASPNETCORE_ENVIRONMENT=Production ;
#启动服务的用户 
user=root ;
stopsignal=INT
redirect_stderr=true
```

创建日志目录

```
mkdir -p /data/logs/demo
```

重启 supervisor

```
systemctl restart supervisord
```

现在可以浏览器继续访问`http://192.168.198.131/WeatherForecast`

退出控制台仍继续运行，并且拥有异常重启日志等多个进程管理功能  

# 3、supervisor UI

supervisor自带UI界面，我们只需要修改一下配置文件即可  

编辑配置文件
```
 vim /etc/supervisord.conf
```
取消以下字段注释：

用户名密码自定义

```
[inet_http_server]
port=0.0.0.0:9001 #默认127.0.0.1::9001只可以本机访问，就不用取消账号密码注释了
username=admin 
password=admin 
```

![IMG](https://gitee.com/imgrep001/m1/raw/master/20200819180626.png)

重启supervisor，浏览器访问`http://192.168.198.131:9001/`

![IMG](https://gitee.com/imgrep001/m1/raw/master/20200819180132.png)





# 4、扩展

如果只是临时的持久运行，也可以使用 `nohup &`

```
nohup dotnet firstdemo.dll --urls http://0.0.0.0:80 &
```

> `nohup` : 加在一个命令的最前面，表示不挂断的运行命令
>
> `&` : 加载一个命令的最后面，表示这个命令放在后台执行

其他相关命令

```sh
#查看端口占用情况
lsof -i:80

#查看dotnet进程
ps -aux|grep dotnet

#查看当前终端后台执行的任务
jobs

#结束jobs查询的任务
fg 1
```



  


>  鸣谢：
>
>  https://www.cnblogs.com/viter/p/10441409.html
>
>  https://www.jb51.net/article/169783.htm