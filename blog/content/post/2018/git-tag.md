+++
author = "Raikay"  
title = "git命令备忘系列(七) - 标签操作（tag）"  
date = "2021-01-13"  
description = "git命令备忘录系列：标签操作（tag） 笔记 学习 教程 分支 差异 版本控制"  
tags = [
    "git",
]  
+++

对于某些提交，我们可以为它打上Tag，表示这次提交很重要， 比如为一些正式发布大版本的 commit，打上TAG，当某个版本出问题了，通过TAG可以快速找到此次提交对应的Hash值， 直接切换到此次版本的代码去查找问题，比起一个个commit找省事多了。

```
git tag        #列出git中现有的所有标签
git tag -r        #查看远程标签
git tag [name]      #创建标签
git tag -d [name]      #删除本地标签
git push origin [name]       # 推送标签到远程仓库
git push origin --tags             # 删除所有本地仓库中不存在的标签
git push origin --delete tag [name]      #删除远程标签
```

在 git 中有两种最主要的标签–轻量级标签（lightweight）和带注释的标签（annotated）。轻量级标签跟分枝一样，不会改变。它就是针对某个特定提交的指针。然而，带注释的标签是git仓库中的对象。它是一组校验和，包含标签名、email、日期，标签信息，GPG签名和验证。一般情况下，建议创建带注释的标签，这样就会保留这些信息，但是如果你只是需要临时性标签或者某些原因你不想在标签中附带上面说的这些信息，lightweight标签更合适些。

**带注释的标签**

在git中创建带注释的标签非常简单，在运行’tag’命令时加上-a就可以了。

```
# 提交
$ git tag -a v1.4 -m ‘version 1.4′

#查看
$ git tag
v0.1
v1.3
v1.4
```

‘-m’指明标签信息，跟标签一起存储。如果你不使用-m指明标签信息，git会自动启动文本编辑器让你输入。
可以使用 git show 命令查看相应标签的版本信息，并连同显示打标签时的提交对象。

```
$ git show v1.4
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