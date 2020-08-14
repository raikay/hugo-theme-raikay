+++
author = "Raikay"
title = "win10 IIS 10.0 无法安装 URL Rewrite Module 重写模块"
date = "2018-03-11"
description = "win10 IIS 10.0 无法安装 URL Rewrite Module 重写模块"
tags = [
    "dotnet",
]

+++



![clipboard](https://gitee.com/imgrep001/m1/raw/master/20200811133744.png)



打开注册表编辑器，在HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\InetStp位置   

把MajorVersion的值改为9之后，就可以安装了，安装完成之后，再把MajorVersion的值改回10，重启一下iis。 