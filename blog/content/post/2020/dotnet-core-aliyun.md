+++
author = "Raikay"
title = " 阿里云容器镜像服务提交代码自动构建Docker镜像"
date = "2020-12-15"
description = "阿里云容器镜像服务构建Docker 部署 dotnet core 项目 应用程序 使用文档说明  简单教程  "
tags = [
    "dotnet",
    "docker",
    '阿里云',
]
weight=97

+++



### 创建命名空间

![IMG](http://blogimg.raikay.com/330634173092073472.png)





### 创建仓库镜像

![IMG](http://blogimg.raikay.com/330634151478824960.png)



如果没有绑定账号，根据提示绑定一下账号，然后选择你的项目即可

![IMG](http://blogimg.raikay.com/330634129479700480.png)


镜像仓库--在对应的镜像 点击 【管理】

![IMG](http://blogimg.raikay.com/330634105102405632.png)

### 添加构建规则

![IMG](http://blogimg.raikay.com/330634078053339136.png)

这个镜像版本号建议 `latest` 在构建的时默认是`latest`

比如

```
# 默认
docker run -p 8000:80 --name mynginx -d nginx 
# 指定镜像版本
docker run -p 8000:80 --name mynginx -d nginx:1
```



现在每次提交代码到master会自动触发构建，也可以手动 立即构建

![IMG](http://blogimg.raikay.com/330634050064748544.png)

