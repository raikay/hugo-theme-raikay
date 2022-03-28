+++
author = "Raikay"
title = "ESP8266包括NodeMcu说明文档和每个版本属性对比图、管脚资料工具 "
date = "2021-01-05"
description = "NodeMcu CP2101 CH340 两种版本的区别，ESP8266包括NodeMcu每个版本属性对比图和管脚说明资料文档工具 ，ESP8266参数图表 对照表 ESP-01 ESP-01S、12E、12F 版本对比 参照,安信可8266在售模块选型表,各个版本之间的区别，NodeMcu管脚说明 文档"
tags = [
    "esp8266",
]

+++

# 规格书/管脚说明/文档

[安信可官网 各类ESP8266模组规格书汇总](https://docs.ai-thinker.com/规格书)

[安信可官网 NodeMcu规格书](https://docs.ai-thinker.com/_media/esp8266/docs/nodemcu8266_v1.2%E8%A7%84%E6%A0%BC%E4%B9%A6.pdf)

[安信可官网 ESP8266资源汇总](https://docs.ai-thinker.com/esp8266)

[Wemos官方文档](https://www.wemos.cc/)

[Arduino-ESP8266官方文档](https://arduino-esp8266.readthedocs.io/)

[个人收集 工具/驱动等资源下载](https://gitee.com/raikay/elefiles)


# ESP8266
### 版本对比图

![IMG](https://raikay.coding.net/p/code/d/m1/git/raw/master/2021/01/05/20210105170518.png)

图片来自安信可官网

# NodeMcu

NodeMcu引脚以及区别见：[CP2102版和CH340版两种NodeMcu的区别和HW-628有什么不同](https://blog.raikay.com/post/2021/nodemcu-cp2102-ch340g-hw628/)

# Wemos D1 R3 
### 对应引脚
```
static const uint8_t PIN_D0 = 3;            //RX
static const uint8_t PIN_D1 = 1;            //TX
static const uint8_t PIN_D2 = 16;           
static const uint8_t PIN_D3_D15 = 5;        //SCL
static const uint8_t PIN_D4_D14 = 4;        //SDA
static const uint8_t PIN_D5_D13 = 14;       //SCK
static const uint8_t PIN_D6_D12 = 12;       //MISO
static const uint8_t PIN_D7_D11 = 13;       //MOSI  
static const uint8_t PIN_D8 = 0;
static const uint8_t PIN_D9_LED = 2;        //LED
static const uint8_t PIN_D10 = 15;          //SS

static const uint8_t PIN_A0 = 17;

static const uint8_t PIN_RX = 3;
static const uint8_t PIN_TX = 1;
static const uint8_t PIN_SCL = 5;
static const uint8_t PIN_SDA = 4;
static const uint8_t PIN_SCK = 114;
static const uint8_t PIN_MISO = 12;
static const uint8_t PIN_MOSI = 13;
static const uint8_t PIN_SS = 15;
static const uint8_t PIN_LED = 2;
```

# Wemos D1 mini
### 引脚图
![](https://raikay.coding.net/p/code/d/m1/git/raw/master/2021/01/21/20210121151010.jpg)
![](https://raikay.coding.net/p/code/d/m1/git/raw/master/2021/01/21/20210121151115.png)

### 引脚对应表

| Pin  | Function                     | ESP-8266 Pin |
| :--- | :--------------------------- | :----------- |
| TX   | TXD                          | TXD          |
| RX   | RXD                          | RXD          |
| A0   | Analog input, max 3.3V input | A0           |
| D0   | IO                           | GPIO16       |
| D1   | IO, SCL                      | GPIO5        |
| D2   | IO, SDA                      | GPIO4        |
| D3   | IO,10k Pull-up               | GPIO0        |
| D4   | IO, 10k pull-up, BUILTIN_LED | GPIO2        |
| D5   | IO, SCK                      | GPIO14       |
| D6   | IO, MISO                     | GPIO12       |
| D7   | IO, MOSI                     | GPIO13       |
| D8   | IO,10k pull-down, SS         | GPIO15       |
| G    | Ground                       | GND          |
| 5V   | 5V                           | –            |
| 3V3  | 3.3V                         | 3.3V         |
| RST  | Reset                        | RST          |

*All IO have interrupt/pwm/I2C/one-wire supported(except D0)*





