+++
author = "Raikay"
title = "git命令备忘系列：配置文件操作（config）"
date = "2018-03-19"
description = "Git 常用命令系列（一）配置文件操作 版本控制 基本操作 代码管理,解决git不自动记录 密码,每次操作都需要输入用户名和密码感觉很繁琐，解决方法，在本地的工程文件夹的.git下打开config文件 添加"
tags = [
    "git",
	"版本控制",
    "配置文件",
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



### 解决git不自动记录 密码

每次操作都需要输入用户名和密码感觉很繁琐，解决方法，在本地的工程文件夹的.git下打开config文件 添加：

```sh
[credential]
     helper = store
```

或者执行这个命令

```sh
git config --global credential.helper store
```