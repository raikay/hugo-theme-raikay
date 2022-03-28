+++
author = "Raikay"
title = "控制继电器 - Arduino开发ESP8266(NodeMcu)学习记录 "
date = "2021-01-16"
description = "控制继电器 - Arduino开发ESP8266(NodeMcu)学习记录，Wemos D1 R1、Wemos D1 R3 Uno、D1 mini、Wifiduino、ESPduino、WiFinfo ESP8266开发板，学习记录，开发教程，文档，示例代码"
tags = [
    "esp8266",
]

+++



### 示例代码：

通过D5引脚（GPIO14输出高低电平）控制继电器开合

```
/**
作者: Raikay (raikay.com)
时间: 2021/01/16
说明: 控制继电器
NodeMcu -   继电器
VU      -    VCC
D5      -    OUT
GND     -    GND
**/
//定义引脚GPIO14，即Nodemcu D5引脚
int relay = 14;
void setup() {
  //设置引脚输出
  pinMode(relay, OUTPUT);
}
void loop() {
  //程序灯带1秒（1000毫秒）
  delay(1000);
  //设置高电平
  digitalWrite(relay, LOW);
  delay(1000);
  //设置低电平
  digitalWrite(relay, HIGH);
}

```

### 继电器

![](https://raikay.coding.net/p/code/d/m1/git/raw/master/2021/01/16/20210116161722.jpg)

### 线路图

继电器是5v供电，NodeMcu两个版本输出5v的引脚不同。

CP2102版 ：Vin引脚输出5v  

CH340版：VU引脚输出5v  

也可以单独5v电源供电。

下图是CH340版



![](https://raikay.coding.net/p/code/d/m1/git/raw/master/2021/01/16/20210116201839.png)



### 实物图

![](https://raikay.coding.net/p/code/d/m1/git/raw/master/2021/01/16/20210116204100.jpg)



### 效果视频

{{< bilibili BV1QT4y1N7f3 >}}