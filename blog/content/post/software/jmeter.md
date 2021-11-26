+++
author = "Raikay"  
title = "jmeter安装使用"  
date = "2021-11-18"  
description = "jmeter安装使用 配置文件说明 中文文档 手册 教程"  
tags = [  
         "jmeter","测试",
]  

+++

## 一、安装jmeter

jmeter3.3以上要匹配8.0以上jdk，所以安装前需要先安装JDK。  

JDK下载地址：  

[https://www.oracle.com/java/technologies/downloads/#java8-windows](https://www.oracle.com/java/technologies/downloads/#java8-windows)

安装后在CMD窗口输入`java -version`,出现版本号即安装成功。  

jmeter下载地址：

[http://jmeter.apache.org/download_jmeter.cgi](http://jmeter.apache.org/download_jmeter.cgi)

配置环境变量:  

```sh
#添加JMETER_HOME，路径就是jmeter保存的位置
JMETER_HOME=D:\Program Files\apache-jmeter-5.4.1
#添加CLASSPATH
CLASSPATH=%JMETER_HOME%\lib\ext\ApacheJMeter_core.jar;%JMETER_HOME%\lib\jorphan.jar;
#修改path，在后面添加
%JMETER_HOME%\bin
```

cmd窗口输入jmeter运行或者双击bin目录下jmeter.bat

![IMG](https://gitee.com/imgrep001/m1/raw/master/2021/11/26/20211126030820.png)



## 二、使用jmeter

> 如果默认没有计划， 点击菜单种文件--新建计划  

### 1、新建线程组

在计划右键--新建线程组    

线程组控制面板:  
线程数：您正在测试的用户数  
加速时间：您希望允许线程组从0到N个用户的时间  
循环计数：应该循环测试的次数  
调度器：测试持续时间，启动延迟     ![IMG](https://gitee.com/imgrep001/m1/raw/master/2021/11/26/20211126031713.png)

### 2、新建http请求

在线程组右键--添加--取样器--http请求

![IMG](https://gitee.com/imgrep001/m1/raw/master/2021/11/26/20211126032551.png)

**post json 请求**

json数据放到消息体数据中    

自定义请求头，在HTTP请求右键--添加--配置原件--HTTP信息头管理器  

![IMG](https://gitee.com/imgrep001/m1/raw/master/2021/11/26/20211126055538.png)

### 3、查看结果

线程组--添加--监听器--[察看结果数 / 汇总报告 / 聚合报告]

![IMG](https://gitee.com/imgrep001/m1/raw/master/2021/11/26/20211126061047.png)
