+++
author = "Raikay"
title = "git命令备忘系列(二) - 配置文件操作（config）"
date = "2021-01-08"
description = "Git 常用命令系列（一）配置文件操作 版本控制 基本操作 代码管理,解决git不自动记录 密码,每次操作都需要输入用户名和密码感觉很繁琐，解决方法，在本地的工程文件夹的.git下打开config文件 添加 版本控制"
tags = [
    "git",
]

+++

###  .gitconfig 配置文件

.giconfig配置文件 共三个

- **system（系统）**：`C:\Program Files\Git\mingw64\etc\gitconfig`，系统中所有用户都会生效
- **global（全局）**：`C:/Users/当前用户/.gitconfig`当前系统用户下生效
- **local（本地）**：`项目路径/.git/config`配置仅在当前项目生效

不同的系统路径可能不一样，可以通过：`git config -e --system` 和`git config -e --global`找到路径  

**优先级**：`local > global > system`

**常用命令：**

```
# 配置
git config --global user.name "用户名"          # 配置用户名
git config --global user.email "用户邮箱"       # 配置邮箱
git config --global core.editor 编辑器          # 配置编辑器，模式使用vi或者vim

# 查看配置
git config --global user.name       # 查看配置的用户名
git config --global user.email      # 查看配置的邮箱

# 查看所有配置列表
git config --global --list      # 查看全局设置相关参数列表
git config --local --list       # 查看本地设置相关参数列表
git config --system --list      # 查看系统配置参数列表
git config --list               # 查看所有Git的配置(全局+本地+系统)

```

### .gitignore 忽略规则配置

该文件位于项目的根目录，主要配置哪些文件/文件夹忽略不提交，且支持正则表达式。

```
# 忽略所有以 .c结尾的文件
*.c

# 但是 不忽略stream.c
!stream.c

# 只忽略当前文件夹下的TODO文件, 不包括其他文件夹下的TODO例如: subdir/TODO
/TODO

# 忽略所有在build文件夹下的文件
build/

# 忽略 doc/notes.txt, 但不包括多层下.txt例如: doc/server/arch.txt
doc/*.txt

# 忽略所有在doc目录下的.pdf文件
doc/**/*.pdf
```

**注：** 有过提交记录的文件再加入到这个规则文件是不起作用的，可以使用以下命令，清楚缓存。

[.]代表所有文件，也可指定具体文件。

```
git rm -r --cached . 
```



### 设置git别名

```
git config –global alias
```

比如 `commit` 定义为 `cm`:

```
git config --global alias.cm commit
```

定义之后就可以这样使用 commit
```
git cm -m 'save'
```



还可以组合多个命令为一个命令，比如将 `add`  `commit` `push` 合并为一个命令 `acp`

```
# 定义
git config --global alias.acp '!f() { git add -A && git commit -m \"$@\" && git push; }; f'

#使用
git acp 'save'
```

也可以直接vim、文本编辑器编辑配置文件  
在`C:\Users\Administrator\.gitconfig`文件中添加上面的代码：

```
[alias]
	cm = commit
	acp = "!f() { git add -A && git commit -m \\\"$@\\\" && git push; }; f"
```
参考demo:
```
alias.p=push
alias.st=status -sb
alias.ll=log --oneline
alias.last=log -1 HEAD --stat
alias.cm=commit -m
alias.rv=remote -v
alias.d=diff
alias.dv=difftool -t vimdiff -y
alias.gl=config --global -l
alias.se=!git rev-list --all | xargs git grep -F
```


### 解决git不自动记录 密码

每次操作都需要输入用户名和密码感觉很繁琐，解决方法，在本地的工程文件夹的.git下打开config文件 添加：

（如果是密码修改过，需要修改windows 凭据：控制面板-->凭据管理器-->Windows凭据 找到对应的站点用户名）

```sh
[credential]
     helper = store
```

或者执行这个命令

```sh
git config --global credential.helper store
```

### 忽略本地文件

添加本地忽略文件

```
git update-index --assume-unchanged 忽略的文件名

```
恢复本地忽略文件

```
git update-index --no-assume-unchanged  忽略的文件名
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