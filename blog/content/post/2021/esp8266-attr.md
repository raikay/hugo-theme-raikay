+++
author = "Raikay"
title = "ESP8266包括NodeMcu说明文档和每个版本属性对比图、管脚资料工具 "
date = "2021-01-05"
description = "ESP8266包括NodeMcu每个版本属性对比图和管脚说明资料文档工具 ，ESP8266参数图表 对照表 ESP-01 ESP-01S、12E、12F 版本对比 参照,安信可8266在售模块选型表,各个版本之间的区别，NodeMcu管脚说明 文档"
tags = [
    "esp8266",
]

+++




### NodeMcu原理图
![IMG](https://gitee.com/imgrep001/m1/raw/master/2021/01/06/20210106153601.png)



### Wemos D1 R3 管脚
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

 ### ESP8266每个版本属性

![IMG](https://gitee.com/imgrep001/m1/raw/master/2021/01/05/20210105170518.png)

图片来自安信可官网


### 规格书/管脚说明

[安信可各类ESP8266模组规格书汇总](https://docs.ai-thinker.com/规格书)

[NodeMcu规格书](https://docs.ai-thinker.com/_media/esp8266/docs/nodemcu8266_v1.2%E8%A7%84%E6%A0%BC%E4%B9%A6.pdf)

[ESP8266资源汇总](https://docs.ai-thinker.com/esp8266)

### 注意事项

1、NodeMcu的IIC（I2C）管脚是GOIO04和GPIO05，就是上面丝印的D1和D2。



