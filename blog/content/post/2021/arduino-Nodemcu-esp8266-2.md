+++
author = "Raikay"
title = "串口调试 - Arduino开发ESP8266(NodeMcu)学习记录 "
date = "2021-01-15"
description = "串口调试 - Arduino开发ESP8266(NodeMcu)学习记录，Wemos D1 R1、Wemos D1 R3 Uno、D1 mini、Wifiduino、ESPduino、WiFinfo ESP8266开发板，学习记录，开发教程，文档，示例代码"
tags = [
    "esp8266",
]

+++


### 示例代码：
```c++
void setup() {
  //初始化串口设置
  Serial.begin(9600);
}

void loop() {
  //获取程序开始运行的毫秒数
  long time = millis();

  //输出调试日志
  Serial.println(time);
}
```
### 选择开发板
![](http://blogimg.raikay.com/330642901837156352.png)
### 选择端口

这个端口 一般可能是 3、5、6根据设备和电脑环境不同有所区别，可以到电脑的设备管理器查看一下

(我机器插了两个板子，所以是下面这种情况，一般只会是COM3或COM5一个)

![](http://blogimg.raikay.com/330642935836184576.png)

![](http://blogimg.raikay.com/330642961765371904.png)
### 上传代码
![IMG](http://blogimg.raikay.com/330642986662760448.png)

### 查看串口调试窗口
【工具】--> 【串口调试】

![](http://blogimg.raikay.com/330643009018400768.png)

![](http://blogimg.raikay.com/330643020963778560.png)

说一下这个9600波特率，和代码上的一致就可以一般是9600、115200  

我也可能会用esp8266系列的其他开发板，其实所有esp8266的板子代码是通用（比如 Wemos D1 R1、Wemos D1 R3 Uno、D1 mini、Wifiduino、ESPduino、WiFinfo），只要对照一下引脚图稍微改动一下差异的引脚就可以。

[引脚图：http://blog.raikay.com/post/2021/esp8266-attr/](http://blog.raikay.com/post/2021/esp8266-attr/)