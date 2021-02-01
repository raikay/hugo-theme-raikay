+++
author = "Raikay"
title = "使用NTP协议获取时间(连接wifi) - Arduino开发ESP8266(NodeMcu)学习记录 "
date = "2021-02-01"
description = "使用NTP协议获取时间(连接wifi) - Arduino开发ESP8266(NodeMcu)学习记录，通过NTP服务器获取当前网络时间 ESP8266连接wifi 使用wifi获取实时北京时间， Wemos D1 R1、Wemos D1 R3 Uno、D1 mini、Wifiduino、ESPduino、WiFinfo ESP8266开发板，学习记录，开发教程，文档，示例代码"
tags = [
    "esp8266",
]

+++
### 所需硬件
ESP8266 开发板（NodeMcu） x1
### 第三方库
```
ESP8266WiFi
```
#### NTP介绍
网络时间协议，英文名称：Network Time Protocol（NTP）是用来使计算机时间同步化的一种协议，它可以使计算机对其服务器或时钟源（如石英钟，GPS等等)做同步化，它可以提供高精准度的时间校正（LAN上与标准间差小于1毫秒，WAN上几十毫秒），且可介由加密确认的方式来防止恶毒的协议攻击。NTP的目的是在无序的Internet环境中提供精确和健壮的时间服务。 ——【百度百科】

### 代码：
```c++
/**
  作者: Raikay (raikay.com)
  时间: 2021/01/30
  说明: 通过NTP服务器获取网络时间（连接wifi）
**/

#include <ESP8266WiFi.h> //wifi库
#include <time.h> //时间库

const char* ssid = "Ziroom211";  //wifi账号
const char* password = "4001001111";  //wifi密码

const char* NTP_SERVER = "ntp.aliyun.com";  //NTP服务器

#define TZ_INFO "UTC-8"  //时区

void setup() {
  Serial.begin(115200);
  Serial.setDebugOutput(true); //将串口设为调试输出模式

  WiFi.disconnect();//断开之前的连接
  WiFi.mode(WIFI_STA);  //设为STA模式
  WiFi.begin(ssid, password);//连接wifi
  Serial.println("\nConnecting to WiFi");

  //连接失败
  while (WiFi.status() != WL_CONNECTED) {
    Serial.print(".");
    delay(1000);
  }

  //设置时区、NTP服务器地址
  configTime(TZ_INFO, NTP_SERVER);
  Serial.println("\nWaiting for time");
  while (!time(nullptr)) {
    Serial.print(".");
    delay(1000);
  }
  Serial.println("");
}

//循环
void loop() {
  time_t now = time(nullptr); //获取当前时间 时间戳

  struct tm *p = localtime(&now); //转换为tm结构

  //格式化输入
  Serial.print(String(1900 + p->tm_year) + "年" + String(1 + p->tm_mon) + "月" + String(p->tm_mday) + "日");
  Serial.println(" " + String(p->tm_hour) + ":" + String(p->tm_min) + ":" + String(p->tm_sec));
  delay(1000);
}
```
![](https://gitee.com/imgrep001/m1/raw/master/2021/01/30/20210130230613.png)


### 串口打印效果
![](https://gitee.com/imgrep001/m1/raw/master/2021/01/30/20210130230644.png)

### tm结构

```
struct tm
{ 
   int tm_sec;       /* 秒 – 取值区间为[0,59] */
   int tm_min;      /* 分 - 取值区间为[0,59] */
   int tm_hour;     /* 时 - 取值区间为[0,23] */
   int tm_mday;     /* 一个月中的日期 - 取值区间为[1,31] */
   int tm_mon;      /* 月份（从一月开始，0代表一月） - 取值区间为[0,11] */
   int tm_year;     /* 年份，其值等于实际年份减去1900 */
   int tm_wday;     /* 星期 – 取值区间为[0,6]，其中0代表星期天，1代表星期一，以此类推 */
   int tm_yday;     /* 从每年的1月1日开始的天数 – 取值区间为[0,365]，其中0代表1月1日，1代表1月2日，以此类推 */
   int tm_isdst;    /* 夏令时标识符，实行夏令时，tm_isdst为正。不实行夏令时，tm_isdst为0；不了解情况时，tm_isdst()为负。*/
};
```

### 常用NTP服务器推荐

```
ntp.aliyun.com #阿里云
time.windows.com  #Windows自带
time.apple.com #苹果macOS自带
time.nist.gov #英特尔
pool.ntp.org  #NTP官方网站
cn.pool.ntp.org  #NTP官方网站中国服务器
cn.ntp.org.cn  #国家授时中心
asia.pool.ntp.org  #洲际空间服务器

查看更多可见：https://tinyurl.com/y6j44elx
```