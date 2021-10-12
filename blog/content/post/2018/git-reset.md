+++
author = "Raikay"
title = "git命令备忘系列(五) - 版本恢复和撤销命令（reset）回滚操作"
date = "2021-01-11"
description = "git命令备忘系列：恢复与撤销 本地（工作区）修改撤销、提交暂存区（commit）撤销、提交到远程仓库撤销、版本回退 "
tags = [
    "git",
]
+++



### 1、撤销本地（工作区）修改

```
git checkout fileName #指定文件撤销
git checkout .   #点表示撤销所有文件
```

checkout 只撤销已有(tracked)文件，新增文件（untracked）不会撤销

**清除工作区新增（untracked）文件**

```sh
git clean -df
```

### 2、撤销暂存区修改（已经add，未commit /push）
把添加（add）到暂存区的更改，恢复到工作区  
```sh
git reset  #撤销暂存区全部文件
git reset  filename #指定文件
```
### 3、查看本地仓库修改(已经commit，未push)



```sh
git log 本地branch ^仓库 远程分支   #可以查看本地有远程没有的提交。
git log 远程分子 ^本地branch   #可以查看远程有，本地没有的提交。
git log master ^origin master #demo
```

### 4、撤销本地仓库修改(已经commit,未push)

仅仅是撤回commit操作，您写的代码仍然保留。  

HEAD^的意思是上一个版本，也可以写成HEAD~1  

如果你进行了2次commit，想都撤回，可以使用HEAD~2  

```
git reset --soft HEAD^
```
**参数说明：**  

- `--mixed `

  不删除工作空间改动代码，撤销commit，并且撤销git add . 操作

- `--soft`

  不删除工作空间改动代码，撤销commit，不撤销git add . 操作

- `--hard`

  删除工作空间改动代码，撤销commit，撤销git add .操作



### 5、远程仓库版本回退（已经push到远程仓库）

本地版本回退到指定的id

```sh
git reset --hard 4e0c318  #4e0c318 是提交的Id,git log/git reflog 等 可以查看
```

强制提交到远程仓库

```sh
git push -f
```

### 6、撤销某次提交

会增加一次修改记录

```
git revert HEAD             # 撤销最近的一个提交
git revert 提交的Hash值     # 撤销某次commit
```

### 7、只修改 commit 注释

```sh
git commit --amend
```

此时会进入默认vim编辑器，修改注释完毕后保存就好了。

#### 相关文章
[git命令备忘系列（一）：基础命令](https://blog.raikay.com/post/2018/git-basic/)  
[git命令备忘系列（二）：配置文件操作（config）](https://blog.raikay.com/post/2018/git-config/)  
[git命令备忘系列（三）：查看历史记录（log）](https://blog.raikay.com/post/2018/git-log/)  
[git命令备忘系列（四）：对比两个分支的差异（diff）](https://blog.raikay.com/post/2018/git-diff/)  
[git命令备忘系列（五）：恢复与撤销（reset）](https://blog.raikay.com/post/2018/git-reset/)  
[git命令备忘系列（六）：分支操作（branch）](https://blog.raikay.com/post/2018/git-branch/)  
[git命令备忘系列（七）：标签操作（tag）](https://blog.raikay.com/post/2018/git-tag/)  
[git命令备忘系列（八）：使用技巧集合](https://blog.raikay.com/post/2018/git-other/)  