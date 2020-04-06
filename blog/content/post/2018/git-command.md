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

### 5、设置git别名

```
git config –global alias
```
比如 `commit` 定义为 `cm`:
```
Raikay@DESKTOP-8CO9PI4 MINGW64 /d/Resources/GitFile/Managix (master)
$ git config --global alias.cm commit

Raikay@DESKTOP-8CO9PI4 MINGW64 /d/Resources/GitFile/Managix (master)
$ git cm -m 'save'
[master 037da09] save
 1 file changed, 2 insertions(+), 1 deletion(-)

```
还可以组合多个命令为一个命令，比如将 `add`  `commit` `push` 合并为一个命令 `acp`

```
Raikay@DESKTOP-8CO9PI4 MINGW64 /d/Resources/GitFile/Managix (master)
$ git config --global alias.acp '!f() { git add -A && git commit -m \"$@\" && git push; }; f'

Raikay@DESKTOP-8CO9PI4 MINGW64 /d/Resources/GitFile/Managix (master)
$ git acp 'save'
[master 51fb37f] "save"
 1 file changed, 1 insertion(+), 2 deletions(-)
Enumerating objects: 10, done.
Counting objects: 100% (10/10), done.
Delta compression using up to 8 threads
Compressing objects: 100% (6/6), done.
Writing objects: 100% (6/6), 582 bytes | 582.00 KiB/s, done.
Total 6 (delta 4), reused 0 (delta 0)
remote: Resolving deltas: 100% (4/4), completed with 4 local objects.
To https://github.com/raikay/Managix.git
   2977f6b..51fb37f  master -> master

```
也可以直接vim、文本编辑器编辑配置文件  
在`C:\Users\Administrator\.gitconfig`文件中添加上面的代码：
```
[alias]
	cm = commit
	acp = "!f() { git add -A && git commit -m \\\"$@\\\" && git push; }; f"
```


### 2、获取帮助 「help」

```
git 命令 -h     
```



然后执行命令 `git acp`



#### 撤销已经commit
```
git reset --soft HEAD^
# HEAD^的意思是上一个版本，也可以写成HEAD~1
# 如果你进行了2次commit，想都撤回，可以使用HEAD~2
```

#### 删除新创建文件
```
git clean -df #删除 一些 没有 git add 的 文件
```

##### 1、远程仓库相关命令

查看远程仓库：`$ git remote -v`

添加远程仓库：`$ git remote add [name] [url]`

删除远程仓库：`$ git remote rm [name]`

修改远程仓库：`$ git remote set-url --push[name][newUrl]`

拉取远程仓库：`$ git pull [remoteName] [localBranchName]`

推送远程仓库：`$ git push [remoteName] [localBranchName]`




##### 2、分支(branch)操作相关命令
查看本地分支：`$ git branch`

查看远程分支：`$ git branch -r`

创建本地分支：`$ git branch [name]` ----注意新分支创建后不会自动切换为当前分支

切换分支：`$ git checkout [name]`

创建新分支并立即切换到新分支：`$ git checkout -b [name]`

删除分支：`$ git branch -d [name]`

*-d选项只能删除已经参与了合并的分支，对于未有合并的分支是无法删除的。如果想强制删除一个分支，可以使用-D选项*

合并分支：`$ git merge [name]`

*将名称为[name]的分支与当前分支合并*

创建远程分支(本地分支push到远程)`$ git push origin [name]`

删除远程分支：`$ git push origin :heads/[name]`

在Git v1.7.0 之后，可以使用这种语法删除远程分支：

```
$ git push origin --delete <branchName>
```

提交本地test分支作为远程的master分支:`$ git push origin test:master`

提交本地test分支作为远程的test分支:`$ git push origin test:test`

强制提交:`$ git push -f origin URL_html:master`

刚提交到远程的test将被删除，但是本地还会保存的，不用担心
```
$ git push origin :test
```
#### 3、忽略一些文件、文件夹 不提交
在仓库根目录下创建名称为`.gitignore`的文件，写入不需要的文件夹名或文件，每个元素占一行即可，如:
```
target
bin
*.db
```

推送`pull`时出现==冲突== 放弃本地修改，使远程库内容强制覆盖本地代码
```
git fetch --all #只是下载代码到本地，不进行合并操作
git reset --hard origin/master  #HEAD指向最新下载的版本
```
#### 4、恢复 撤销
```
$ git checkout -- readme.txt
```

命令`git checkout -- readme.txt`意思就是，把readme.txt文件在工作区的修改全部撤销，这里有两种情况：
一种是readme.txt自修改后还没有被放到暂存区，现在，撤销修改就回到和版本库一模一样的状态；
一种是readme.txt已经添加到暂存区后，又作了修改，现在，撤销修改就回到添加到暂存区后的状态。
总之，就是让这个文件回到最近一次git commit或git add时的状态。
`git checkout .` 只是一个点，是全部撤销

回退所有内容到上一个版本  
```
git reset HEAD^  
```
#### 查看本地仓库（commit） 未提交到(push)远程仓库的代码
```
#git log 本地branch ^远程分支   可以查看本地有远程没有的提交。
#git log 远程分子 ^本地branch   可以查看远程有，本地没有的提交。
git log master ^origin master
```
---



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


```
**追问：**

我的本地分支是master，远程分支也是master，
使用`git log master ^master` 没有任何响应。是我理解错误了吗？

**追答:**

本地分支是自己建立的分支如master，远程分支一般是origin/XXX，这个仓的远程库。

你自己提交代码是先add，然后commit。这个时候是提交在自己的本地分支。git push或者repo upload的命令执行的是往中心库的提交。

就比如你吃饭。中心库就是锅里的。远程分支是盘子里的。本地分支是碗里的。你所有操作都是在操作本地分支的。

**********************************************

本地修改了许多文件，其中有些是新增的，因为开发需要这些都不要了，想要丢弃掉，可以使用如下命令：
```
git checkout . #本地所有修改的。没有的提交的，都返回到原来的状态
git stash #把所有没有提交的修改暂存到stash里面。可用git stash pop回复。

git reset --hard HASH #返回到某个节点，不保留修改。
git reset --soft HASH #返回到某个节点。保留修改

git clean -df #删除 一些 没有 git add 的 文件；
git clean 参数
    -n 显示 将要 删除的 文件 和  目录
    -f 删除 文件
    -df 删除 文件 和 目录
也可以使用：
git checkout . && git clean -xdf




每次操作都需要输入用户名和密码感觉很繁琐，解决方法，在本地的工程文件夹的.git下打开config文件 添加：
```
[credential]
     helper = store
```
或者执行这个命令

```
git config --global credential.helper store
```
再输入一次用户名密码后就可以保存住了。


http://www.oschina.net/translate/10-tips-git-next-level

http://jser.me/2014/06/20/Git%E5%AD%A6%E4%B9%A0%E6%80%BB%E7%BB%93.html