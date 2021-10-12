+++
author = "Raikay"  
title = "DotNet知识点总结"  
date = "2021-03-16"  
description = "DotNet知识点总结 .net 学习路线图 脑图 目录"  

+++
## 第一部分：.Net基础

1. **.Net基础**：数据类型、变量、运算符、分支结构、循环结构、方法、反编译器、递归、递归算法的非递归优化；
2. **面向对象**：异常、封装继承多态、单例模式、装饰者设计模式、this本质论、static、namespace、类型转换、is与as、抽象类、接口、宫廷系统案例、foreach、随机数及案例；
3. **常用类库**：String与StringBuilder、可空类型、文件操作（File、Directory、FileStream、StreamReader、StreamWriter）、常用数据结构（List、Dictionary、Set、Queue、Stack等）；

## 第二部分：数据库开发 

1. **SQL语言**：基础语句（Select、Delete、Insert、Update）、Where、聚合函数、排序与分组、联合查询、外键约束、子查询、MySQL数据库、SQLServer数据库；
2. **ADO.Net**：基础类、SQL注入漏洞与参数化查询、离线结果集、事务、ADO.Net中的多态编程、海量数据高速插入（SQLServer、MYSQL两套方案）；
3. **高级数据库操作**：MySQL命令行操作、DML（Create Table、Alter Table等）、having、相关子查询、数据库安全控制、视图、存储过程、触发器；

## 第三部分：.Net高级技术 

1. **高级特性**：多项目开发、CLR、CTS、CLS、IL与程序集、索引器、密闭类、静态类与扩展方法、深拷贝和浅拷贝、结构体、值类型与引用类型、拆箱装箱、字符串拘留池、ref与out、正则表达式、XML、序列化；
2. **委托与事件**：委托语法、内置委托Func和Action、匿名方法、lambda表达式、lambda的推演、lambda原理探秘、常用扩展方法、事件本质论；
3. **反射**：反射、实现通用对象拷贝、Attribute及案例、自动动手写ORM引擎；
4. **三层架构**：三层架构的原理、代码生成器、项目案例；

## 第四部分：Web前端 

1. **HTML与CSS**：基本标签、li与ol、表单、框架、div、HTML5；常用选择器、常用样式、盒子模型、定位方式；
2. **Javascript**：基础语法、json、神奇的Array、常用Javascript类、JS的调试技巧；
3. **JS Dom**：节点的获取、元素的操作、节点创建、事件与冒泡、项目案例；
4. **JQuery**：隐式迭代、选择器、JQuery如何实现JSDom中的效果、JQuery EasyUI；

## 第五部分：ASP.Net核心编程 

1. **Web底层原理**：Socket编程、自己编写浏览器、自己编写WebServer、Http协议、HttpHandler、核心对象（Request、Response、Server、Application）
2. **ASP.Net深入**：不用控件的ASP.Net、上传下载、验证码、网站开发安全防范、Cookie与Session、自己编写Session类、分布式Session；
3. **ASP.Net高级**：狂虐WebForm、AJAX、Json、JQuery AJAX、ServerPush、Global、UrlRewrite、缓存、笨重的母版页与轻量级的SSI、网站部署与IIS配置；

## 第六部分：ASP.net MVC 

1. **EF基础**：C#6.0新语法、Nuget、var与类型推断、匿名类、dynamic、Entity Framework的使用、三种EF开发模式、linq、EF性能优化、EF本质论、SQL监控、EF中执行原生SQL、导航属性与lazyload；主要讲解目前最流行的FluentAPI方式配置CodeFirst；EF对象状态转换；EF关系配置秘诀（一对多、多对多）；EF实体继承；
2. **ASP.Net MVC：**：Razor语法详解；分页、数据传输方式（ViewBag、ViewData、TempData、Model）、各种ActionResult、四种Filter（IAuthorizationFilter、IActionFilter、IResultFilter、IExceptionFilter）、HtmlHelper、路由机制、验证、layout；

## 第七部分：掌上租项目 

1. 这是一个使用ASP.Net MVC+Entity Framework(FluentAPI CodeFirst)+AutoFac等技术开发的互联网项目，全程采用TDD开发流程。主要的技术有：
2. **前端技术**：前端MVC引擎(artTemplate)、HUI、MUI（手机端自适应）、ValidForm、Layer；
3. **.Net高级技术**：自定义Filter、自定义ModelBinder、ASP.Net MVC+EntityFramework最佳实践；
4. **大型网站架构**：UnitTest、AutoFac、分布式架构（Redis、Memecached等）、CDN与云存储、云计算服务（短信验证、SendCloud邮件云）、RBAC权限控制、页面静态化和SSI；数据库并发控制（悲观锁与乐观锁）；
5. **高级开源组件**：ElasticSearch全文搜索引擎；Quartz.Net定时调度；UEditor；Log4Net最新版；互联网网站安全（XSS、CSRF等）；CodeCarvings.Piczard（水印、缩略图）；CaptchaGen（验证码）；WebUploader文件无刷新上传；

## 第八部分：.Net core+Linux 

1. **Linux**：“microsoft love linux”战略背后的意义、Linux的安装、Linux常用命令、vim编辑器、Linux文件系统和文件操作、Linux的系统管理、Linux上部署开发环境和运行环境；

2. **.Net core**：.Net core的战略意义、对比.Net Framework学.Net core、.Net core开发环境的搭建、如何在Linux下运行.Net core网站、Nginx、对比Entity Framework学Entity Framework Core、对比ASP.Net MVC学ASP.Net MVC Core、MySql数据库、EF Core+MySql；

3. **Docker**：什么是Docker、Docker与Devops、Docker在云计算架构中的应用、.Net Core开发中Docker的应用；

## 第九部分：.Net并发编程 

1. **多线程**：Thread、参数化Thread、线程同步、线程池、多线程中的异常处理、多线程的陷阱、多线程的局限性；

2. **TPL异步编程**：TPL与传统APM模型的区别、async与await、异步IO、Entity Framework的异步操作、ASP.Net MVC的异步操作；

## 第十部分：NoSQL 

1. **MongoDB**：MongoDB的优缺点、MongoDB的增删改查、.Net 操作MongoDB、MongoDB应用案例分析；

2. **Redis**：Redis的优缺点；Redis常用数据类型（String、List、Set、Sorted Set）、.Net 操作Redis、Redis应用案例分析；

3. **Memcached**：Memcached介绍、Memcached 集群、Memcached应用案例分析；

## 第十一部分：ASP.Net MVC深入 

1. **Web API**：Web API优点；什么是Restful；移动互联网时代多终端开发架构；基于Token的Web API认证；Web API的多版本管理；

2. **ASP.Net MVC其他**：路由深入；HtmlHelper；

## 其他 
源代码版本管理系统；  
Bug管理系统；  
网络支付；  
分布式日志框架；  
阿里云、Azure等云服务器的使用；  
微信小程序开发；  