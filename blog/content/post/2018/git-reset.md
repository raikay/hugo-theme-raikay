+++
author = "Raikay"
title = "git命令备忘系列：恢复与撤销"
date = "2018-03-11"
description = "git命令备忘系列：恢复与撤销 本地（工作区）修改撤销、提交暂存区（commit）撤销、提交到远程仓库撤销、版本回退"
tags = [
    "git",
	"版本控制",
]

+++



### 1、撤销本地（工作区）修改

```
git checkout fileName #指定文件撤销
git checkout .   #点表示撤销所有文件
```

checkout 只撤销已有(tracked)文件，新增文件（untracked）不会撤销

清除工作区新增（untracked）文件

```sh
git clean -df
```

### 2、撤销暂存区修改（已经add，未commit /push）

```sh
git reset --soft HEAD^
```
`HEAD^`的意思是上一个版本，也可以写成`HEAD~1`

如果你进行了2次commit，想都撤回，可以使用`HEAD~2`
