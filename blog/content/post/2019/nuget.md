+++
author = "Raikay"
title = "NuGet源中国加速镜像，博客园，华为等"
date = "2019-03-11"
description = "NuGet源中国加速镜像，博客园，华为、官方镜像等 如何修改NuGet源"
tags = [
    "dotnet",
]

+++



打开Visual Studio => 工具 => NuGet包管理器 => 程序包管理器设置

![20200407123245](https://raikay.coding.net/p/code/d/m1/git/raw/master/20200811133529.png)

![image-20200407123814591](https://raikay.coding.net/p/code/d/m1/git/raw/master/20200811133554.png)



**NuGet微软官方中国镜像地址：**  
https://nuget.cdn.azure.cn/v3/index.json

**官方默认源：**

https://api.nuget.org/v3/index.json

**博客园：**

https://nuget.cnblogs.com/v3/index.json

**华为：**

https://repo.huaweicloud.com/repository/nuget/v3/index.json

**MyNuGet：**  
https://dotnet.myget.org/F/dotnet-core/api/v3/index.json  

这是一个很激进的 NuGet 源，包含各种日构建包（其中包括 .NET Standard 或者 .NET Core 等库的日构建版本），所以如果你希望尝试最新的 API 最新的功能，最好设置此 NuGet 源。

**早期版本：**

https://www.nuget.org/api/v2/
https://nuget.org/api/v2/

