+++
author = "Raikay"
title = "Node.js包管理工具Yarn、npm安装以及常用命令"
date = "2020-12-14"
description = "NodeJs包管理工具Yarn、npm安装以及常用命令"
tags = [
    "前端","Node.js","Yarn","npm"
]

+++


### npm
NodeJs自带的工具，无需单独安装  

```shell
# 安装nodejs
yum install -y nodejs

# 查看安装的版本
node -v 
# 安装n管理包，安装指定的nodejs版本
npm install -g n 
# 更新NodeJs版本

# 安装最新版本
n latest 
# 安装指定定版本
n 8.11.3  
```

如果某些项目编译时提示安装Visual C ++ Build Tools 2017、.net framework 4.5.1、python2.7

执行命令：

```
npm install --global --production windows-build-tools
```

#### yarn的安装:

1. 下载node.js，使用npm安装  
```sh
npm install -g yarn
#查看版本：
yarn --version
```
2. yarn 淘宝源安装，分别复制粘贴以下代码行到黑窗口运行即可  
   yarn config set registry `https://registry.npm.taobao.org -g`  
   yarn config set sass_binary_site `http://cdn.npm.taobao.org/dist/node-sass -g ` 


鸣谢：  
> [https://blog.csdn.net/yw00yw/article/details/81354533](https://blog.csdn.net/yw00yw/article/details/81354533)
> [https://www.cnblogs.com/liuxuande/p/14004627.html](https://www.cnblogs.com/liuxuande/p/14004627.html)
