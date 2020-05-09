+++
author = "Raikay"
title = "git命令备忘系列（一）：基础命令"
date = "2018-06-06"
description = "git 常用命令 笔记 学习 教程 分支 差异."
tags = [
    "git",
	"版本控制",
]

+++



### 1、克隆项目到本地

```
git clone https://github.com/raikay/gittest.git
```
### 2、拉取最新

```
git pull 
```

### 3、添加文件到暂存区

```
git add 文件名  # 指定文件。
git add .      # 将当前工作区的所有文件都加入暂存区
```

### 4、将缓存区的内容提交到本地仓库

```
git commit -m "提交说明"
git commit --amend    #追加/修改上次提交、不新增提交记录
```

### 5、查看工作区与缓存区的状态「git status」

```
git status
```

### 6、推送到远程分支

```sh
git push
git push origin branch1 #多个仓库时，指定origin仓库下 branch1分支
```

### 7、获取帮助 「help」

```
git 命令 -h     
```

### 8、git pull 强制覆盖本地

```sh
// 从远程仓库下载最新版本
git fetch -all 
// 将本地设为刚获取的最新的内容
git reset --hard origin/master
```
### 9、本地已有项目 初始化操作

1. 初始化基础文件：`git init`  
2. 添加到暂存区：`git add .`  
3. 提交到本地仓库：`git commit -m 'init'`  
4. 添加远程远程仓库地址：`git remote add origin https://github.com/raikay/gittest.git`
5. 推送到远程仓库：`git push -u origin master`    

>`-u`：创建 upStream上传流，只在初次推送代码时创建，以后就不用加`-u`参数了



#### 相关文章

