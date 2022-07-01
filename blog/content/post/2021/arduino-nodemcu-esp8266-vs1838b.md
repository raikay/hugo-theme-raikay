+++
author = "Raikay"
title = "接收红外信号VS1838B - Arduino开发ESP8266(NodeMcu)学习记录 "
date = "2021-01-17"
description = "红外接收头VS1838B - Arduino开发ESP8266(NodeMcu)学习记录，NodeMcu开发板 红外线信号接收 红外接收头VS1838B， Wemos D1 R1、Wemos D1 R3 Uno、D1 mini、Wifiduino、ESPduino、WiFinfo ESP8266开发板，学习记录，开发教程，文档，示例代码"
tags = [
    "esp8266",
]

+++



### 所需硬件

VS1838B 红外接收头

ESP8266 开发板 （NodeMcu）

红外信号遥控器  

杜邦线 x3

### 第三方库

IRremoteESP8266

下载地址：[https://github.com/crankyoldgit/IRremoteESP8266](https://github.com/crankyoldgit/IRremoteESP8266)

### 红外接收头图片

![](http://blogimg.raikay.com/330639344920563712.png)



### 线路图

![](http://blogimg.raikay.com/330639328583749632.jpg)

### 实物图

![](http://blogimg.raikay.com/330639292835696640.jpg)

### 代码

```c++
/**
  作者: Raikay (raikay.com)
  时间: 2021/01/17
  说明: NodeMcu使用红外接收头接收红外信号
  NodeMcu -   VS1838B
  3v3     -    VCC
  GND     -    GND
  D5      -    OUT
**/

#include <Arduino.h>
#include <IRrecv.h>
#include <IRremoteESP8266.h>
#include <IRac.h>
//#include <IRtext.h>
#include <IRutils.h>

// 定义接收信号的引脚GPIO14，即NodeMcu的D5
const uint16_t kRecvPin = 14;

// (接收引脚，接收信号大小，超时时间)
IRrecv irrecv(kRecvPin, 1024, 50, true);

// 存储结果
decode_results results;

void setup() {

  Serial.begin(115200);

#if DECODE_HASH
  // 过滤未知信号值
  irrecv.setUnknownThreshold(12);
#endif
  irrecv.setTolerance(kTolerance);
  // 启动接收器
  irrecv.enableIRIn();
}

void loop() {
  // 是否收到消息
  if (irrecv.decode(&results)) {
    
    // 接收到的编码
    Serial.print("输出编码：");
    serialPrintUint64(results.value, HEX);
    Serial.println();
    
    // 协议和编码
    Serial.println("输出协议和编码：");
    Serial.println(resultToHumanReadableBasic(&results));
    yield();

    // 说明
    String description = IRAcUtils::resultAcToString(&results);
    Serial.println("输出说明：");
    if (description.length())
    {
      Serial.println("Mesg Desc.: " + description);
    } else
    {
      Serial.println("Mesg Desc.: 无");
    }

    // 原数据
    Serial.println("输出原数据：");
    Serial.println(resultToSourceCode(&results));
    Serial.println("------------------------分割线------------------------");
    yield();
  }
}
```



### 使用的红外遥控器  

![](http://blogimg.raikay.com/330639240113295360.jpg)

对应的编码：

| 关闭：FFA25D |           | MENU：FFE21D |
| :----------: | :-------: | :----------: |
| TEST：FF22DD | +：FF02FD | 返回：FFC23D |
| \|<<：FFE01F | >：FFA857 | >>\|：FF906F |
|  0：FF6897   | -：FF9867 |  c：FFB04F   |
|  1：FF30CF   | 2：FF18E7 |  3：FF7A85   |
|  4：FF10EF   | 5：FF38C7 |  6：FF5AA5   |
|  7：FF42BD   | 8：FF4AB5 |  9：FF52AD   |



 ### 接收结果  

上传程序之后直接打开串口监视器查看即可。

可以看出来，Code是`FF02FD`、`FF906F`，协议是NEC，现在市面上NEC是使用比较多的一种协议。  

rawData是原数据，在无法解码的情况，可以使用原数据直接发出，同样可以控制。  

![](http://blogimg.raikay.com/330639218076422144.png)

### 通过红外线信号控制板载LED灯代码
```c++
/**
  作者: Raikay (raikay.com)
  时间: 2021/01/17
  说明: NodeMcu使用红外接收头接收红外信号
  NodeMcu -   VS1838B
  3v3     -    VCC
  GND     -    GND
  D5      -    OUT
**/

#include <Arduino.h>
#include <IRrecv.h>
#include <IRremoteESP8266.h>
//#include <IRac.h>
//#include <IRtext.h>

#include <IRutils.h>

// 定义接收信号的引脚GPIO14，即NodeMcu的D5
const uint16_t kRecvPin = 14;

// (接收引脚，接收信号大小，超时时间)
IRrecv irrecv(kRecvPin, 1024, 50, true);

// 存储结果
decode_results results;


void setup() {

  Serial.begin(115200);
  pinMode(LED_BUILTIN, OUTPUT);
  digitalWrite(LED_BUILTIN, HIGH);

#if DECODE_HASH
  // 过滤未知信号值
  irrecv.setUnknownThreshold(12);
#endif
  irrecv.setTolerance(kTolerance);
  // 启动接收器
  irrecv.enableIRIn();
}



void loop() {
  // 是否收到消息
  if (irrecv.decode(&results)) {
    String codeStr = uint64ToString(results.value, HEX);

    //信号Code等于FFA857 打开Led
    if (codeStr ==  "FFA857")
    {
      Serial.println("打开Led");
      digitalWrite(LED_BUILTIN, LOW);
    }
    ///信号Code等于FFA25D 关闭Led
    if (codeStr == "FFA25D")
    {
      Serial.println("关闭Led");
      digitalWrite(LED_BUILTIN, HIGH);
    }
    yield();
  }
}
```


### 效果视频

{{< bilibili BV1uz4y1S7MR >}}


### 其他红外信号接收结果
格力空调 红外遥控器 接收结果  

协议：`GREE `

编码：`0x3C0E205010200070`、`0x3C0E205010200070`、`0x3C0D205010200060`

![](http://blogimg.raikay.com/330639196140212224.png)


美的空调 省电星 红外接收结果

![](http://blogimg.raikay.com/330639177794326528.png)



长虹电视 红外遥控器接收结果

![](http://blogimg.raikay.com/330639159347777536.png)

### 参数图片

![](http://blogimg.raikay.com/330639140360163328.png)
