+++
author = "Raikay"
title = "NodeJs包管理工具Yarn、npm安装以及常用命令"
date = "2020-12-14"
description = "NodeJs包管理工具Yarn、npm安装以及常用命令"
tags = [
    "前端",
]

+++


### npm
NodeJs自带的工具，无需单独安装

#### yarn的安装:

1. 下载node.js，使用npm安装
```
npm install -g yarn
查看版本：yarn --version
```
2. yarn 淘宝源安装，分别复制粘贴以下代码行到黑窗口运行即可
   yarn config set registry `https://registry.npm.taobao.org -g`
   yarn config set sass_binary_site `http://cdn.npm.taobao.org/dist/node-sass -g`


转自：
```
https://blog.csdn.net/yw00yw/article/details/81354533
```