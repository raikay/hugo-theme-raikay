+++
author = "Raikay"  
title = "dotnet项目执行shell脚本实现简单的自动化部署"  
date = "2021-09-18"  
description = "dotnet项目执行shell脚本实现简单的自动化部署,通过HTTP WEB请求调用执行shell sh脚本实现简单的自动化部署 CI/CD"  
tags = [  
    "dotnet", "CentOS","Linux","jenkins","Docker", 
]  

+++

不要k8s、不要docker、不要Jenkins，只要一个部署脚本，只是一个小项目单台服务器，实现提交代码自动执行脚本，拉代码构建部署项目。

创建一个web api 项目，作为webhook,实现接收web请求后执行shell脚本

![IMG](http://blogimg.raikay.com/330626858540470272.png)



项目代码：  
```c#
using Microsoft.AspNetCore.Mvc;
using Microsoft.Extensions.Logging;
using System;
using System.Diagnostics;

namespace ShellHandler.Controllers
{
    [ApiController]
    [Route("[controller]")]
    public class HandlerController : ControllerBase
    {
        private readonly ILogger<HandlerController> _logger;

        public HandlerController(ILogger<HandlerController> logger)
        {
            _logger = logger;
        }

        [HttpPost]
        public string Execute(string fileName)
        {
            try
            {
                var processStartInfo = new ProcessStartInfo($"./{fileName}") { RedirectStandardOutput = true };
                var process = Process.Start(processStartInfo);
                if (process == null)
                {
                    Console.WriteLine("Can not run shell .");
                }
                else
                {
                    using (var sr = process.StandardOutput)
                    {
                        while (!sr.EndOfStream)
                        {
                            var str = sr.ReadLine();
                            Console.WriteLine(str);
                        }

                        if (!process.HasExited)
                        {
                            process.Kill();
                        }
                    }
                }
                return "ok";

            }
            catch (Exception ex)
            {
                _logger.LogInformation(ex.Message);
                return ex.Message;
            }
        }
    }
}

```

在服务器部署当前项目

```
nohup dotnet ShellHandler.dll --urls http://0.0.0.0:8080 &
```

在项目根目录创建部署脚本`publish.sh`
```sh
#!/bin/bash
#杀死占用8081端口的进程
kill -9 $(lsof -i:8081 -t)
cd /home/web/web-demo/
#拉取代码
git pull
#发布项目到publish文件夹
dotnet publish -o publish
cd publish/
#后台运行项目
nohup dotnet WebDemo.dll --urls http://0.0.0.0:8081 &
```

添加执行权限

```shell
chmod a+x publish.sh
```

创建一个Demo项目部署到服务器，托管到gitee  

![IMG](http://blogimg.raikay.com/330626893743263744.png)

![IMG](http://blogimg.raikay.com/330619443430428672.png)


把webhook地址添加到gitee的WebHooks，并指定脚本文件名
> 大多数git平台都有webhook功能   

![IMG](http://blogimg.raikay.com/330627389509996544.png)



项目做一些改动，git提交代码  
![IMG](http://blogimg.raikay.com/330627583022600192.png)  

查看gitee触发记录  

![IMG](http://blogimg.raikay.com/330619475013537792.png)

查看网站已经完成自动部署

![IMG](http://blogimg.raikay.com/330627066452119552.png)

### 相关文章：
[Docker环境安装及基础命令使用](https://blog.raikay.com/post/2020/docker/)  
[.Net Core项目使用Docker容器部署到Linux服务器](https://blog.raikay.com/post/2020/dotnet-docker/)  
[Linux系统Centos7部署DotNet Core项目及环境安装 ](https://blog.raikay.com/post/2019/dotnet-publish/)   
[dotnet项目执行shell脚本实现简单的自动化部署](https://blog.raikay.com/post/dotnet/easy-ci-cd/)  
[jenkins实现dotnet项目持续集成、持续部署（CI/CD）](https://blog.raikay.com/post/dotnet/jenkins/)  
[阿里云容器镜像服务提交代码自动构建Docker镜像](https://blog.raikay.com/post/2020/dotnet-core-aliyun/)  
