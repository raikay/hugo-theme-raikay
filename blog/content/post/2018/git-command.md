+++
author = "Raikay"
title = "git 常用命令汇总"
date = "2018-03-11"
description = "git 常用命令 笔记 学习 教程 分支 差异."
tags = [
    "git",
	"版本控制",
]
+++





### 1、初始化仓库

**本地已有项目：**

1. 初始化基础文件：`git init`  
2. 添加到暂存区：`git add .`  
3. 提交到本地仓库：`git commit -m 'init'`  
4. 添加远程远程仓库地址：`git remote add origin https://github.com/raikay/gittest.git`
5. 推送到远程仓库：`git push -u origin master`    

>`-u`：创建 upStream上传流，只在初次推送代码时创建，以后就不用加`-u`参数了

**本地没有项目：**

```
git clone https://github.com/raikay/gittest.git
```

### 2、添加文件到暂存区「add」

```
git add 文件名  # 指定文件。
git add -u     # 即update，不处理新文件
git add .      # 将当前工作区的所有文件都加入暂存区
git add -i     # 进入交互界面模式，按需添加文件到缓存区
```

### 3、将缓存区的内容提交到本地仓库

```
git commit -m "提交说明"
git commit -a -m "提交说明" # 直接把工作区以及缓存区内容提交到本地仓库，不推荐使用，不处理新文件
git commit --amend    #追加/修改上次提交、不新增提交记录
```

### 4、查看工作区与缓存区的状态「git status」

```
git status     
git status -s   # 让结果以更简短的形式输出
```

### 5、推送到远程分支

```sh
git push
git push origin branch1 #多个仓库时，指定origin仓库下 branch1分支
```



### 6、查看某个文件是谁改动的「git blame」

```sh
git blame 文件名 # 查看某文件的每一行内容的作者，最新commit和提交时间
```



### 7、获取帮助 「help」

```
git 命令 -h     
```



然后执行命令 `git acp`





### 8、git pull 强制覆盖本地

```sh
// 从远程仓库下载最新版本
git fetch -all 
// 将本地设为刚获取的最新的内容
git reset --hard origin/master
```





##### 1、远程仓库相关命令

查看远程仓库：`$ git remote -v`

添加远程仓库：`$ git remote add [name] [url]`

删除远程仓库：`$ git remote rm [name]`

修改远程仓库：`$ git remote set-url --push[name][newUrl]`

拉取远程仓库：`$ git pull [remoteName] [localBranchName]`

推送远程仓库：`$ git push [remoteName] [localBranchName]`




##### 2、分支(branch)操作相关命令
提交本地test分支作为远程的master分支:`$ git push origin test:master`

提交本地test分支作为远程的test分支:`$ git push origin test:test`

强制提交:`$ git push -f origin URL_html:master`

刚提交到远程的test将被删除，但是本地还会保存的，不用担心
```
$ git push origin :test
```
推送`pull`时出现==冲突== 放弃本地修改，使远程库内容强制覆盖本地代码
```
git fetch --all #只是下载代码到本地，不进行合并操作
git reset --hard origin/master  #HEAD指向最新下载的版本
```


#### 7、子模块(submodule)相关操作命令
添加子模块：`$ git submodule add [url] [path]`
如：
```
$ git submodule add git://github.com/soberh/ui-libs.git src/main/webapp/ui-libs
```

初始化子模块：`$ git submodule init` ----只在首次检出仓库时运行一次就行

更新子模块：`$ git submodule update` ----每次更新或切换分支后都需要运行一下

##### 删除子模块：（分4步走哦）

1. `$ git rm --cached [path]`

2. 编辑`.gitmodules`文件，将子模块的相关配置节点删除掉

3. 编辑`.git/config`文件，将子模块的相关配置节点删除掉

4. 手动删除子模块残留的目录

   


也可以使用：
```
git checkout . && git clean -xdf
```




http://www.oschina.net/translate/10-tips-git-next-level

http://jser.me/2014/06/20/Git%E5%AD%A6%E4%B9%A0%E6%80%BB%E7%BB%93.html