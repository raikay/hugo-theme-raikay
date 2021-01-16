+++
author = "Raikay"
title = "控制继电器 - Arduino开发ESP8266(NodeMcu)学习记录 "
date = "2021-01-16"
description = "串口调试 - Arduino开发ESP8266(NodeMcu)学习记录，Wemos D1 R1、Wemos D1 R3 Uno、D1 mini、Wifiduino、ESPduino、WiFinfo ESP8266开发板，学习记录，开发教程，文档，示例代码"
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

### 线路图

![](https://gitee.com/imgrep001/m1/raw/master/2021/01/16/20210116154300.png)

### 实物图

![](https://gitee.com/imgrep001/m1/raw/master/2021/01/16/20210116154703.png)

### 效果视频

{{< bilibili BV1bf4y1k7uL >}}