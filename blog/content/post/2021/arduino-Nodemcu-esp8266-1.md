+++
author = "Raikay"
title = "快速开始- Arduino开发ESP8266(NodeMcu)学习记录 "
date = "2021-01-14"
description = "快速开始（环境安装、点亮LED灯）- Arduino-IDE开发ESP8266(NodeMcu)学习记录，Wemos D1 R1、Wemos D1 R3 Uno、D1 mini、Wifiduino、ESPduino、WiFinfo ESP8266开发板，学习记录，开发教程，文档，示例代码"
tags = [
    "esp8266",
]

+++




# 环境安装

### 下载Arduino-IDE
官网：https://www.arduino.cc/  
下载地址：https://www.arduino.cc/en/software  
下载时不是一定要付费，选择`JUST DOWNLOAD
`免费下载  

![IMG](https://raikay.coding.net/p/code/d/m1/git/raw/master/2021/01/05/20210105221202.png)

解压后双击EXE就可以打开了  

### 安装驱动
驱动下载地址：[https://gitee.com/raikay/elefiles/tree/main/tools/驱动](https://gitee.com/raikay/elefiles/tree/main/tools/%E9%A9%B1%E5%8A%A8)

### 下载ESP8266库

文件-->首选项
把这个网址填到 【附加开发版管理器】

```
http://arduino.esp8266.com/stable/package_esp8266com_index.json
```

![IMG](https://raikay.coding.net/p/code/d/m1/git/raw/master/2021/01/05/20210105222351.png)


工具--> 开发板-->开发板管理器

![IMG](https://raikay.coding.net/p/code/d/m1/git/raw/master/2021/01/05/20210105223116.png)

如果没有梯子下载可能会非常慢，可以用下面的直接安装工具

工具下载地址：[https://gitee.com/raikay/elefiles/tree/main/开发板/esp8266](https://gitee.com/raikay/elefiles/tree/main/%E5%BC%80%E5%8F%91%E6%9D%BF/esp8266)



# 点亮LED

### 新建项目
![IMG](https://raikay.coding.net/p/code/d/m1/git/raw/master/2021/01/14/20210114205657.png)


### 代码

```c++
//定义引脚GPIO14，即Nodemcu D5引脚
int led = 14; 
void setup() {
  //设置led引脚输出
  pinMode(led, OUTPUT);
}
void loop() {
  //程序等待1秒（1000毫秒）
  delay(1000);
  //设置led引脚高电平
  digitalWrite(led, LOW); 
  delay(1000);
  //设置led引脚低电平
  digitalWrite(led, HIGH); 

}
```


### 线路图
![IMG](https://raikay.coding.net/p/code/d/m1/git/raw/master/2021/01/14/20210114213459.png)

### 实物图
![IMG](https://raikay.coding.net/p/code/d/m1/git/raw/master/2021/01/14/20210114214048.jpg)



### 选择开发板

![](https://raikay.coding.net/p/code/d/m1/git/raw/master/2021/01/15/20210115213751.png)

### 选择端口

这个端口 一般可能是 3、5、6根据设备和电脑环境不同有所区别，可以到电脑的设备管理器查看一下

(我机器插了两个板子，所以是下面这种情况，一般只会是COM3或COM5一个)

![](https://raikay.coding.net/p/code/d/m1/git/raw/master/2021/01/15/20210115215331.png)

![](https://raikay.coding.net/p/code/d/m1/git/raw/master/2021/01/15/20210115214415.png)

### 上传代码

![IMG](https://raikay.coding.net/p/code/d/m1/git/raw/master/2021/01/15/20210115215810.png)

### 运行效果
![IMG](http://imgrep001.gitee.io/m1/2021/01/14/20210114214940.gif)



我也可能会用esp8266系列的其他开发板，其实所有esp8266的板子代码是通用（比如 Wemos D1 R1、Wemos D1 R3 Uno、D1 mini、Wifiduino、ESPduino、WiFinfo），只要对照一下引脚图稍微改动一下差异的引脚就可以。

[引脚图：http://blog.raikay.com/post/2021/esp8266-attr/](http://blog.raikay.com/post/2021/esp8266-attr/)