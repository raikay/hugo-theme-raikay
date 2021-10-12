+++
author = "Raikay"
title = "git命令备忘系列(三) - 查看提交历史记录（log）"
date = "2021-01-09"
description = "git常用命令查看历史记录（git log） 笔记 学习 教程 分支 差异 ."
tags = [
    "git",
]

+++


```
git log                 # 查看所有commit记录(SHA-A校验和，作者名称，邮箱，提交时间，提交说明)
git log -p -次数                # 查看最近多少次的提交记录
git log --stat                  # 简略显示每次提交的内容更改
git log --name-only             # 仅显示已修改的文件清单
git log --name-status           # 显示新增，修改，删除的文件清单
git log --oneline               # 让提交记录以精简的一行输出
git log –graph –all --online    # 图形展示分支的合并历史
git log --author=作者           # 查询作者的提交记录(和grep同时使用要加一个--all--match参数)
git log --grep=过滤信息         # 列出提交信息中包含过滤信息的提交记录
git log -S查询内容              # 和--grep类似，S和查询内容间没有空格
git log fileName              # 查看某文件的修改记录，找背锅专用
git blame 文件名             # 查看某文件的每一行内容的作者，最新commit和提交时间
```
除此之外，还可以通过 –pretty 对提交信息进行定制，比如：


### 占位符：

| 占位符    | 说明                                       | 占位符    | 说明                           |
| --------- | ------------------------------------------ | --------- | ------------------------------ |
| `%H`  | 提交对象（commit）的完整哈希字串           | `%h`  | 提交对象的简短哈希字串         |
| `%T`  | 树对象（tree）的完整哈希字串               | `%t`  | 树对象的简短哈希字串           |
| `%P`  | 父对象（parent）的完整哈希字串             | `%p`  | 父对象的简短哈希字串           |
| `%an` | 作者（author）的名字                       | `%ae` | 作者的电子邮件地址             |
| `%ad` | 作者修订日期（可以用 –date= 选项定制格式） | `%ar` | 按多久以前的方式显示           |
| `%cn` | 提交者(committer)的名字                    | `%ce` | 提交者的电子邮件地址           |
| `%cd` | 提交日期                                   | `%cr` | 提交日期，按多久以前的方式显示 |
| `%s`  | 提交说明                                   |           |                                |

### 参数：



| 选项                 | 说明                                                         |
| -------------------- | ------------------------------------------------------------ |
| `-p`             | 按补丁格式显示每个更新之间的差异                             |
| `–stat`          | 显示每次更新的文件修改统计信息(行数)                         |
| `–shortstat`     | 只显示 –stat 中最后的行数修改添加移除统计                    |
| `–name-only`     | 仅在提交信息后显示已修改的文件清单                           |
| `–name-status`   | 显示新增、修改、删除的文件清单                               |
| `–abbrev-commit` | 仅显示 SHA-1 的前几个字符，而非所有的 40 个字符              |
| `–relative-date` | 使用较短的相对时间显示（比如，“2 weeks ago”）                |
| `–graph`         | 显示 ASCII 图形表示的分支合并历史                            |
| `–pretty`        | 格式定制，可选选项有：oneline，short，full，Fullerton和format(后跟指定格式) |

### 限制log输出参数：

| 选项                  | 说明                               |
| --------------------- | ---------------------------------- |
| `-(n)`            | 仅显示最近的 n 条提交              |
| `–since, –after`  | 仅显示指定时间之后的提交。         |
| `–until, –before` | 仅显示指定时间之前的提交。         |
| `–author`         | 仅显示指定作者相关的提交。         |
| `–committer`      | 仅显示指定提交者相关的提交。       |
| `–grep`           | 仅显示含指定关键字的提交           |
| `-S`              | 仅显示添加或移除了某个关键字的提交 |



### 示例：
**示例1：**

```
$ git log --pretty=format:"%h was committed by %an , %ar"
2977f6b was committed by raikay , 11 days ago
7fc1672 was committed by raikay , 2 weeks ago
519be01 was committed by raikay , 2 weeks ago
```

**示例2：**

```
$ git log --pretty=format:"%h - %an, %ar : %s"
2977f6b - raikay, 11 days ago : 精简路由格式
7fc1672 - raikay, 2 weeks ago : 格式化
519be01 - raikay, 2 weeks ago : 菜单管理功能完善
```

**指定文件示例：**

```
$ git log --pretty=format:"%h - %an, %ar : %s" Raikay.Managix.API/Controllers/MenuController.cs
2977f6b - raikay, 11 days ago : 精简路由格式
519be01 - raikay, 2 weeks ago : 菜单管理功能完善
c06ce9d - raikay, 2 weeks ago : 前期调整
```



### 查看某次提交的修改内容「git show」

```sh
git show 提交Hash值     # 查看某次commit的修改内容
```



### 查看所有HEAD变动记录， 如commit， 分支切换信息

```sh
git reflog
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