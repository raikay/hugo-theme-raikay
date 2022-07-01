+++
author = "Raikay"
title = "DHT11温湿度传感器 - Arduino开发ESP8266(NodeMcu)学习记录 "
date = "2021-01-18"
description = "DHT11/DHT22温湿度传感器 - Arduino-IDE开发ESP8266(NodeMcu)学习记录，NodeMcu开发板 ， Wemos D1 R1、Wemos D1 R3 Uno、D1 mini、Wifiduino、ESPduino、WiFinfo ESP8266开发板，学习记录，开发教程，文档，示例代码"
tags = [
    "esp8266",
]

+++



### 所需硬件

ESP8266 开发板（NodeMcu）  x1

DHT11温湿度传感器  x1

面包板  x1

杜邦线  若干

### 第三方库

DHT

可以直接安装  

项目-->加载库-->管理库--> 搜索"DHT"安装  

或者手动下载  

下载地址：[https://github.com/adafruit/DHT-sensor-library](https://github.com/adafruit/DHT-sensor-library)

### 线路图

![](http://blogimg.raikay.com/330638870272151552.png)

### 实物图  

![](http://blogimg.raikay.com/330638834251468800.jpg)



### 代码



```c++
/**
  作者: Raikay (raikay.com)
  时间: 2021/01/18
  说明: ESP8266接收DHT11温湿度传感器数据
  NodeMcu       DHT11
  3V3       -    正极
  D5        -    out
  GND       -    负极

**/
#include <DHT.h>
#define DHTPIN     14     //GPIO14 即NodeMcu D5
#define DHTTYPE   DHT11     // 定义你DHT11
DHT dht(DHTPIN, DHTTYPE);
//读取DHT的温度值
String readDHTTemperature() {
  float t = dht.readTemperature();
  if (isnan(t)) {
    Serial.println("Failed to read from DHT sensor!");
    return "--";
  }
  else {
    return String(t);
  }
}
//读取DHT的湿度值
String readDHTHumidity() {
  float h = dht.readHumidity();
  if (isnan(h)) {
    Serial.println("Failed to read from DHT sensor!");
    return "--";
  }
  else {
    return String(h);
  }
}
void setup() {
  pinMode(DHTPIN, OUTPUT);
  digitalWrite(DHTPIN, HIGH);
  Serial.begin(9600);
  dht.begin();
}
void loop() {
  //温度  temperature
  Serial.print("温度:");
  Serial.println(readDHTTemperature().c_str());
  //湿度  Humidity
  Serial.print("湿度:");
  Serial.println(readDHTHumidity().c_str());
  delay(1000);
  Serial.println("---------------");
}
```

### 串口显示
![IMG](http://blogimg.raikay.com/330638781235466240.png)