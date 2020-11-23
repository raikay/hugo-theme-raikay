+++
author = "Raikay"  
title = "git命令备忘系列（八）：其他命令集合"  
date = "2018-08-06"  
description = "ggit命令备忘系列（八）：其他命令集合  笔记 学习 教程 分支 差异 "  
tags = [
    "git",
]  
+++



### 1、Git彻底删除一次提交

这样删除之后，在历史记录中也查看不到

```
git rebase --onto SHA^ SHA
```

SHA是错误的提交。 

例如，删除`cbe8527`这次提交:
```
git rebase --onto cbe8527^ cbe8527
```
这条记录就从历史记录中消失了

### 2、git stash 情况处理


两个分支都没有这个文件，创建文件之后，可以两个分支随意切换！

两个分支都有这个文件，发生改变之后，也可以随意切换

A分支没有这个文件，B分支有这个文件，在B分支对这个文件进行更改，想切换A分支
切换失败并且提示：

```sh
$ git checkout master

error: Your local changes to the following files would be overwritten by checkout:
        stu1_create1.cs
Please, commit your changes or stash them before you can switch branches.
Aborting
```
##### 解决方案1
将当前工作状态保存
```sh
git stash save "work in progress for foo feature" 
#返回到自己上一个commit
```
pull, edit, etc 或者fix 紧急bug之后 
```
$git stash pop  #继续原来的工作
```
现在 之前提交的代码也在本地

>###### 如果有未提交的行数冲突 pop 会失败
```sh
$ git stash pop
error: Your local changes to the following files would be overwritten by merge:
        stu1_create1.cs
Please, commit your changes or stash them before you can merge.
Aborting
```
>###### 如果有提交的行数冲突 会提示：
```
$ git stash pop
Auto-merging stu1_create1.cs
CONFLICT (content): Merge conflict in stu1_create1.cs

```
代码内部状态：
```
<<<<<<< Updated upstream
				//第五行注释2017年8月28日22:32:59
=======
				//插入第四行注释2017年8月28日22:29:59
>>>>>>> Stashed changes
```
查看当前所有stash
```sh
$ git stash list
stash@{0}: On stu_git_1: bug8
stash@{1}: On stu_git_1: bug3
stash@{2}: On stu_git_1: bug2
stash@{3}: On stu_git_1: bug1

```
应用指定stash,应用之后不删除stash

```
	$ git stash apply stash@{1}

```
恢复指定stash,恢复之后stash内容不存在

```
$ git stash pop stash@{1}

```
在创建C分支，在C分支中可以直接 apply stu_git_1 分支的stash



### 3、git cherry-pick用法 重演commit


#### 场景: 
>如果你的应用已经发布了一个版本2.0, 代码分支叫release-2.0, 现在正在开发3.0, 代码的分支叫dev-3.0. 那么有一天产品说, 要把正在开发的某个特性提前上线, 也就是说要把dev-3.0分支上的某些更改移到2.x的版本上, 那么怎么办呢?


该cherry-pick上场了, cherry-pick会重演某些commit, 即把某些commit的更改重新执行一遍. 那么上述问题的解决方案如下:


基于release-2.0分支新建分支release-2.1, 并且到新创建的分支上

`git checkout -b release-2.1 release-2.0`

将dev-3.0分支上的某些commit在release-2.1分支上重演

`git cherry-pick dev-3.0`分支的某些commit-hash

如:
```
git cherry-pick  20c2f506d789bb9f041050dc2c1e954fa3fb6910 
```
多个commit-hash使用空格分割, commit-hash最好按提交时间先后排列, 即最先提交的commit放在前面.

cherry-pick不仅可以用在不同分支之间, 还可以用在同一个分支上.

不同分支的用法如上所述. 同一分支用法也是一样的, 同一分支使用情形:

比如说你在某一个向某个分支中添加了一个功能, 后来处于某种原因把它给删除了,

然而后来某一天你又要添加上这个功能了, 这时候就可以使用cherry-pick把添加那个功能的commit, 再重演一遍.

### 4、强制覆盖本地代码（与git远程仓库保持一致）

```
 git fetch --all &&  git reset --hard origin/master && git pull
```



*鸣谢：*

```
http://www.jianshu.com/p/d577dcc36a08
http://blog.csdn.net/wh_19910525/article/details/7784901
```