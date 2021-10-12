+++
author = "Raikay"  
title = "git命令备忘系列(八) - 使用技巧集合"  
date = "2021-01-15"  
description = "ggit命令备忘系列(八) - 其他命令集合  笔记 学习 教程 分支 差异 "  
tags = [
    "git",
]  

+++



# 一、Git彻底删除一次提交

这样删除之后，在历史记录中也**查不到**

```sh
git rebase --onto SHA^ SHA
```

SHA是错误的提交。 

例如，删除`cbe8527`这次提交:
```sh
git rebase --onto cbe8527^ cbe8527
```
这条记录就从历史记录中消失了

# 二、git stash 工作状态保存

##### 解决方案1

将当前工作状态保存
```sh
git stash save "保存说明" 
```
恢复保存内容
```sh
git stash pop
```
如果有冲突， pop恢复时会失败，需要处理冲突  

查看当前所有stash
```sh
#查看命令
$ git stash list

#结果如下：
stash@{0}: On stu_git_1: bug8
stash@{1}: On stu_git_1: bug3
stash@{2}: On stu_git_1: bug2
stash@{3}: On stu_git_1: bug1

```
恢复指定stash,恢复之后不删除stash

```sh
$ git stash apply stash@{1}
```
恢复指定stash,恢复冰删除stash

```sh
$ git stash pop stash@{1}
```
# 三、git cherry-pick用法 重演commit


#### 场景: 
>如果应用已经发布了一个版本2.0, 代码分支叫release-2.0, 现在正在开发3.0, 代码的分支叫dev-3.0. 那么有一天产品说, 要把正在开发的某个特性提前上线, 也就是说要把dev-3.0分支上的某些更改移到2.x的版本上, 那么怎么办呢?


该cherry-pick上场了, cherry-pick会重演某些commit, 即把某些commit的更改重新执行一遍，不限制分支， 那么上述问题的解决方案如下:


基于release-2.0分支新建分支release-2.1, 并且到新创建的分支上

`git checkout -b release-2.1 release-2.0`

将dev-3.0分支上的某些commit在release-2.1分支上重演

`git cherry-pick dev-3.0`分支的某些commit-hash

如:
```sh
git cherry-pick  20c2f506d789bb9f041050dc2c1e954fa3fb6910 
```
多个commit-hash使用空格分割, commit-hash最好按提交时间先后排列, 即最先提交的commit放在前面.

cherry-pick不仅可以用在不同分支之间, 还可以用在同一个分支上.

不同分支的用法如上所述. 同一分支用法也是一样的, 同一分支使用情形:

比如说你在某一个向某个分支中添加了一个功能, 后来处于某种原因把它给删除了,

然而后来某一天你又要添加上这个功能了, 这时候就可以使用cherry-pick把添加那个功能的commit, 再重演一遍.

# 四、强制覆盖本地代码（与git远程仓库保持一致）

```sh
 git fetch --all &&  git reset --hard origin/master && git pull
```

# 五、递归克隆

项目里包含的一些库或者一些模块是存在了别的仓库，可以用递归来克隆回来。一次性就能解决所有的依赖模块

```sh
git clone --recursive https://github.com/dotnet/aspnetcore.git
```

# 六、Git设置代理/使用SS/SSR代理


```shell
# 设置ss
git config --global http.proxy 'socks5://127.0.0.1:1080'
git config --global https.proxy 'socks5://127.0.0.1:1080'

# 设置代理
git config --global https.proxy http://127.0.0.1:1080
git config --global https.proxy https://127.0.0.1:1080

# 取消代理
git config --global --unset http.proxy
git config --global --unset https.proxy
```
> 注意端口要换成自己的配置，默认为1080





#### 相关文章

[git命令备忘系列（一）：基础命令](https://blog.raikay.com/post/2018/git-basic/)  
[git命令备忘系列（二）：配置文件操作（config）](https://blog.raikay.com/post/2018/git-config/)  
[git命令备忘系列（三）：查看历史记录（log）](https://blog.raikay.com/post/2018/git-log/)  
[git命令备忘系列（四）：对比两个分支的差异（diff）](https://blog.raikay.com/post/2018/git-diff/)  
[git命令备忘系列（五）：恢复与撤销（reset）](https://blog.raikay.com/post/2018/git-reset/)  
[git命令备忘系列（六）：分支操作（branch）](https://blog.raikay.com/post/2018/git-branch/)  
[git命令备忘系列（七）：标签操作（tag）](https://blog.raikay.com/post/2018/git-tag/)  
[git命令备忘系列（八）：使用技巧集合](https://blog.raikay.com/post/2018/git-other/)  



#### 扩展

[Git大全](https://gitee.com/all-about-git)