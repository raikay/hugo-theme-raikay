+++
author = "Raikay"
title = "DotNet平台中 IL / CTS / CLS / CLR 都是什么？"
date = "2021-03-12"
description = "DotNet平台中 IL / CTS / CLS / CLR 都是什么？名词解释 原理剖析，这些词到第是什么意思"
tags = [
    "dotnet",
]

+++


### IL(MSIL): 
可以执行的二进制代码(托管代码)  

### CTS:
.Net平台通用数据类型(Common Type System)  

### CLS：
.Net平台通用语言规范（语法）(Common Language Specification )  

### CLR:
公共语言运行时( Common Language Runtime )  

### 关系
各个语言（VB.Net、F#）编译器把自己语言按照CTS、CLS编程成可以在CLR上运行的IL 

![IMG](https://gitee.com/imgrep001/m1/raw/master/2021/03/12/20210312151913.png)