+++
author = "Raikay"  
title = "C#中字符串类型String与string大小写两种类型的区别"  
date = "2020-12-13"  
description = "C#中字符串类型String与string大小写两种类型的区别dotnet core"  

+++

1. `string`是c#中的类，`String`是`.net Framework`的类(在c# IDE中不会显示蓝色)  
2. C# `string`映射为`.net Framework`的`String`  
3. 如果用`string`,编译器会把它编译成`String`，所以如果直接用String就可以让编译器少做一点点工作。如果使用c#，建议使用`string`，比较符合规范  
4. `String`始终代表 `System.String(1.x)` 或 `::System.String(2.0)` ，`String`只有在前面有`using System`;的时候并且当前命名空间中没有名为`String`的类型（`class`、`struct`、`delegate`、`enum`）的时候才代表`System.String`  
5. `string`是关键字，`String`不是，也就是说`string`不能作为类、结构、枚举、字段、变量、方法、属性的名称，而`String`可以。  