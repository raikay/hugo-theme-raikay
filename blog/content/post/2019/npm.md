+++
author = "Raikay"
title = "npm和cnpm的各种源以及切换管理工具nrm"
date = "2019-09-15"
description = "npm和cnpm的各种源以及切换管理工具nrm 国内源 国外 切换"
tags = [
    "前端",
]

+++

### npm

- 允许用户从NPM服务器下载第三方包到本地使用。
- 允许用户从NPM服务器下载并安装第三方命令行程序到本地使用。
- 允许用户将自己编写的包或命令行程序上传到NPM服务器供别人使用

### npm命令

- `npm -v` 来测试是否成功安装
- 查看当前目录已安装插件：`npm list`
- 更新全部插件： `npm update [ --save-dev ]`
- 使用 npm 更新对应插件： `npm update <name> [ -g ] [ --save-dev]`
- 使用 npm 卸载插件： `npm uninstall <name> [ -g ] [ --save-dev ]`

### cnpm

- 淘宝团队做的国内镜像，因为npm的服务器位于国外可能会影响安装。淘宝镜像与官方同步频率目前为 10分钟 一次以保证尽量与官方服务同步。
- 安装：命令提示符执行
   `npm install cnpm -g --registry=https://registry.npm.taobao.org`
- `cnpm -v` 来测试是否成功安装

### 通过改变地址来使用淘宝镜像

- npm的默认地址是`https://registry.npmjs.org/`
- `npm config get registry`查看npm的仓库地址
- `npm config set registry https://registry.npm.taobao.org`改变默认下载地址，这样不安装`cnpm`就能采用淘宝镜像  

### nrm

- `nrm`能够管理所用可用的镜像源地址以及当前所使用的镜像源地址，但是只是单纯的提供了几个url并能够让我们在这几个地址之间方便切换  

- `nrm`包安装命令： `npm i nrm -g `   


- `nrm ls`：查看所有可用的镜像，并可以切换。*号表示当前npm使用的地址，可以使用命令  

  ` nrm use taobao`或 `nrm use npm`来进行两者之间的切换。  


```
$ nrm ls
npm -------- https://registry.npmjs.org/  
yarn ------- https://registry.yarnpkg.com/  
cnpm ------- http://r.cnpmjs.org/  
taobao ----- https://registry.npm.taobao.org/  
nj --------- https://registry.nodejitsu.com/  
npmMirror -- https://skimdb.npmjs.com/registry/  
edunpm ----- http://registry.enpmjs.org/  
  
```

  

**参数说明**

- `-g`：全局安装。 将会安装在`C:\Users\Administrator\AppData\ Roaming\npm`，**并且写入系统环境变量**；非全局安装：将会安装在当前定位目录;全局安装可以通过命令行任何地方调用它，本地安装将安装在定位目录的`node_modules`文件夹下，通过要求调用;  

- `-S`：即`npm install module_name --save`,写入`package.json`的`dependencies` ,`dependencies` 是需要发布到生产环境的，比如jq，vue全家桶，ele-ui等ui框架这些项目运行时必须使用到的插件就需要放到`dependencies`    

  

- `-D`：即`npm install module_name --save-dev`,写入`package.json`的`devDependencies` ,`devDependencies` 里面的插件只用于开发环境，不用于生产环境。比如一些babel编译功能的插件、webpack打包插件就是开发时候的需要，真正程序打包跑起来并不需要的一些插件。

  

> 为什么要保存在`package.json`  因为node_module包实在是太大了。用一个配置文件保存，只打包安装对应配置文件的插件，按需导入。




转载：https://www.jianshu.com/p/115594f64b41
