+++
author = "Raikay"
title = "git命令备忘系列(四) - 对比两个分支的差异（diff）"
date = "2021-01-10"
description = "git 常用命令 笔记 学习 教程 分支 差异 ."
tags = [
    "git",
]

+++

#### 1、工作区与缓存区的差异

```
git diff
```

#### 2、工作区与某分支的差异

远程分支这样写：`remotes/origin/branchNmae`

```
git diff branchNmae
```

#### 3、工作区与HEAD指针指向的内容差异	

```
git diff HEAD
```

示例：

```sh
git diff HEAD^             # 工作区与上次提交内容差异
git diff HEAD^^            # 上上次提交内容差异
git diff HEAD~3            # 也可以直接~次数
git diff head f42b15f5     # 也可以指定id
```

#### 4、工作区某文件“当前版本”与“历史版本”的差异

```
git diff 提交id 文件路径
```

#### 5、查看从某个版本后所有改动内容

```
git diff 版本TAG
```

#### 6、 查看两个分支指定文件的详细差异
```
git diff branch1 branch2 具体文件路径
```    

#### 7、查看所有有差异的文件的详细差异(也支持TAG)
```
git diff branch1 branch2
```  
#### 8、查看 branch1 分支有，而 branch2 中没有的 log
```
git log branch1 ^branch2
```  

#### 9、查看 branch2 中比 branch1 中多提交了哪些内容

列出来的是两个点后边（此处即 branch2）多提交的内容。

```
git log branch1..branch2
```

#### 10、不知道谁提交的多谁提交的少，单纯想知道有什么不一样

```
git log branch1...branch2
```  
#### 11、在上述情况下，显示出每个提交是在哪个分支上

commit 后面的箭头，根据我们在 –left-right branch1…branch2 的顺序，左箭头 <表示 branch1 的，右箭头> 表示branch2 的。

```
git log --left-right branch1...branch2
```  
示例：  

```
$ git log --left-right master...dev
```

 *注：如果只想统计哪些文件被改动，多少行被改动，可以添加--stat参数*  



#### 相关文章
[git命令备忘系列（一）：基础命令](https://blog.raikay.com/post/2018/git-basic/)  
[git命令备忘系列（二）：配置文件操作（config）](https://blog.raikay.com/post/2018/git-config/)  
[git命令备忘系列（三）：查看历史记录（log）](https://blog.raikay.com/post/2018/git-log/)  
[git命令备忘系列（四）：对比两个分支的差异（diff）](https://blog.raikay.com/post/2018/git-diff/)  
[git命令备忘系列（五）：恢复与撤销（reset）](https://blog.raikay.com/post/2018/git-reset/)  
[git命令备忘系列（六）：分支操作（branch）](https://blog.raikay.com/post/2018/git-branch/)  
[git命令备忘系列（七）：标签操作（tag）](https://blog.raikay.com/post/2018/git-tag/)  
[git命令备忘系列（八）：使用技巧集合](https://blog.raikay.com/post/2018/git-other/)  