+++
author = "Raikay"
title = "git命令备忘系列(六) - 创建合并等分支操作（branch）"
date = "2021-01-12"
description = "git命令备忘系列：分支操作（branch）查看分支列表、 创建并切换新分支、强制创建分支、新创建的分支推送到远程仓库、恢复误删除分支、分支重命名、合并分支、解决冲突 版本控制"
tags = [
    "git",
]

+++

### 1、查看分支列表
```sh
git branch  #显示本地分支
git branch -r #显示远程所有分支
```

### 2、创建分支和切换分支

```sh
git branch branchName    # 创建分支
git checkout branchName1   #切换到 branchName1 分支
git checkout -b branchName2   #创建并切换到 branchName2分支
```

**强制创建分支**

```sh
git checkout -B branchName
```

为什么强制创建分支，比如已经有dev分支，打算创建新的dev分支，正常情况同名不允许创建，`-B`参数之后既可以成功创建

**新创建的分支推送到远程仓库**

```sh
git push --set-upstream origin branchName
git push origin branch1 #多个仓库时，指定origin仓库下 branch1分支
```

### 3、删除

```
git branch -d 分支名    # 删除分支，分支上有未提交更改是不能删除的
git branch -D 分支名    # 强行删除分支，尽管这个分支上有未提交的更改
git push origin --delete 分支名   #删除远程分支
```

### 4、恢复误删除分支

两步：找出被删分支最新的commit的Hash值，然后恢复分支：

```
git log --branches="被删除的分支名"     # 找到被删分支最新的commit版本号
git branch 分支名 版本号(前七位即可)    # 恢复被删分支
```

### 5、分支重命名

```sh
git branch -m 老分支名 新分支名     # 分支重命名
```

### 6、合并分支

branchName1 合并到当前分支

```
git merge branchName1
```
**常用参数**  
```sh
git merge -ff           # 快速合并，默认参数
git merge -ff-only      # 只有快速合并的情况才合并
git merge --no-ff       # 不使用快速合并
git merge -n 分支名     # 合并分支，不会在合并后显示合并前后的不同状态
git merge -stat 分支名  # 合并分支，合并结束后显示合并前后的不同状态
git merge -e 分支名     # 合并分支，合并前调用编辑器，可自行编写commit
```

### 7、解决冲突

冲突后会进入临时的冲突分支 `branchName | MERGE`   

`<<<<` 和 `>>>>`包裹着的就是冲突内容,以`====`分隔开，上半部分是当前分支的代码  

保留想要的代码，然后删除 `<<<<` / `>>>>` / `====`  

最后:  

```sh
git add .
git commit -m '注释'
```

提交后就会自动跳出临时的冲突分支

### 8、远程仓库相关命令

查看远程仓库：`$ git remote -v`

添加远程仓库：`$ git remote add [name] [url]`

删除远程仓库：`$ git remote rm [name]`

修改远程仓库：`$ git remote set-url --push[name][newUrl]`

拉取远程仓库：`$ git pull [remoteName] [localBranchName]`

推送远程仓库：`$ git push [remoteName] [localBranchName]`

## 9、删除远程已不存在的本地分支

解决方法：  
```
git fetch --prune    #这样就可以实现在本地删除远程已经不存在的分支  
```
或者简略：  
```
 git fetch -p
 git fetch -p origin
 git remote prune origin
```

#### 相关文章
[git命令备忘系列（一）：基础命令](https://blog.raikay.com/post/2018/git-basic/)  
[git命令备忘系列（二）：配置文件操作（config）](https://blog.raikay.com/post/2018/git-config/)  
[git命令备忘系列（三）：查看历史记录（log）](https://blog.raikay.com/post/2018/git-log/)  
[git命令备忘系列（四）：对比两个分支的差异（diff）](https://blog.raikay.com/post/2018/git-diff/)  
[git命令备忘系列（五）：恢复与撤销（reset）](https://blog.raikay.com/post/2018/git-reset/)  
[git命令备忘系列（六）：分支操作（branch）](https://blog.raikay.com/post/2018/git-branch/)  
[git命令备忘系列（七）：标签操作（tag）](https://blog.raikay.com/post/2018/git-tag/)  
[git命令备忘系列（八）：使用技巧集合](https://blog.raikay.com/post/2018/git-other/)  