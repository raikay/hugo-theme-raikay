+++
author = "Raikay"
title = "Dto / Model / Entity 区别"
date = "2020-02-11"
description = "数据 实体 传输 Dto / Model / Entity 有什么区别 怎么区分使用"
tags = [
    "dotnet",
]

+++

## Entity
数据表对应到实体类的映射


## Model

Model是MVC中一个概念，可能不和Entity一一对应，因为展示在View层中数据可能是一个Entity的精简，也可能是多个Entity的组合。
Model是一个高度优化组合或者精简后的一个用于在View层展示数据的对象。


## DTO
数据传输对象（Data Transfer Object）。
用于系统间数据传输的对象，数据传输目标往往是数据访问对象从数据库中检索数据。

鸣谢：
> https://www.zhihu.com/question/25256772  
> http://www.liuhuachao.com/blog/2018/05/20/entity-model-dto/  

