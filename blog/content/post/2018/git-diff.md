+++
author = "Raikay"
title = "git命令备忘系列：对比两个分支的差异（git diff）"
date = "2018-04-08"
description = "git 常用命令 笔记 学习 教程 分支 差异."
tags = [
    "git",
    "版本控制",
    "差异",
]

+++





#### 1、工作区与缓存区的差异

`git diff`

示例：

```yaml
$ git diff
diff --git a/Raikay.Managix.API/Controllers/MenuController.cs b/Raikay.Managix.API/Controllers/MenuController.cs
index 9d75053..34d7733 100644
--- a/Raikay.Managix.API/Controllers/MenuController.cs
+++ b/Raikay.Managix.API/Controllers/MenuController.cs
@@ -45,7 +45,8 @@ namespace Raikay.Managix.API.Controllers
         [HttpPost]
         public async Task<dynamic> Post(MenuDto param)
         {
-            return await _currentService.Add(param);
+                       var result=await _currentService.Add(param);
+            return result;
         }
         [HttpDelete]
         public async Task<dynamic> Delete(List<Guid> param)

```



#### 2、工作区与某分支的差异

远程分支这样写：`remotes/origin/branchNmae`

```
git diff branchNmae
```

#### 3、工作区与HEAD指针指向的内容差异	

`git diff HEAD`

示例：

```sh
git diff HEAD^             # 工作区与上次提交内容差异
git diff HEAD^^            # 上上次提交内容差异
git diff HEAD~3            # 也可以直接~次数
git diff head f42b15f5     # 也可以指定id
```

#### 4、工作区某文件“当前版本”与“历史版本”的差异

`git diff 提交id 文件路径`

#### 5、查看从某个版本后所有改动内容

`git diff 版本TAG`

#### 6、 查看两个分支指定文件的详细差异
`
git diff branch1 branch2 具体文件路径
`    

#### 7、查看所有有差异的文件的详细差异(也支持TAG)
`
git diff branch1 branch2
`  
#### 8、查看 branch1 分支有，而 branch2 中没有的 log
`
git log branch1 ^branch2
`  

#### 9、查看 branch2 中比 branch1 中多提交了哪些内容

列出来的是两个点后边（此处即 branch2）多提交的内容。

`
git log branch1..branch2
`

#### 10、不知道谁提交的多谁提交的少，单纯想知道有什么不一样

`
git log branch1...branch2
`  
#### 11、在上述情况下，显示出每个提交是在哪个分支上

commit 后面的箭头，根据我们在 –left-right branch1…branch2 的顺序，左箭头 <表示 branch1 的，右箭头> 表示branch2 的。

`
git log --left-right branch1...branch2
`
示例：
```
$ git log --left-right master...dev
commit < 2977f6b32b65588c4165b4dcd5a8f3dc079736c9 (HEAD -> master, origin/master, origin/HEAD)
Author: raikay <raikay@163.com>
Date:   Thu Mar 26 20:37:32 2020 +0800

    精简路由格式

commit < 7fc167256788e90879b3b3135b3dcaa45536ee08
Author: raikay <raikay@163.com>
Date:   Sun Mar 22 23:52:46 2020 +0800

    格式化


```

 *注：如果只想统计哪些文件被改动，多少行被改动，可以添加--stat参数*  




