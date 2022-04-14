+++
author = "Raikay"  
title = "jQuery给编译后页面中vue变量赋值"  
date = "2022-04-14"  
description = "JQ给编译后页面中vue变量赋值"  
tags = [  
         "JQ,vue,jQuery,js" ,
]  

+++

要赋值的input框

```
<input tabindex="1" type="text" autocomplete="off" placeholder="AppID" name="appId" class="el-input__inner">
```

通过name获取对象赋值

```
$("input[name='appId']").val("24563574-04f5-4a44-848e-84ea81677e34");
```

赋值后没有效果，需要触发vue事件

```
$("input[name='appId']")[0].dispatchEvent(new Event('input'))
```

