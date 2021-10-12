+++
author = "Raikay"
title = "CP2102版和CH340版两种NodeMcu的区别和HW-628有什么不同"
date = "2021-01-21"
description = "NodeMCU开发版介绍  CP2102版和CH340版两种NodeMcu的区别和HW-628有什么不同   有哪些不同  引脚和管脚的差异"
tags = ["esp8266","NodeMcu","CH340","CP2102","区别"]

+++

NodeMcu 常见的有两个版本，一般用通讯模块区分   

**CP2102版** : NodeMcu V1.0 ，作者是amica  

**CH340版** : NodeMcu v3，作者是Lolin  

v3不是V1.0的升级版，可能是两个分支。  

**区别：**  

1、外观上v3比V1.0 大一些。  

2、CP2102版 A0附近两个RSV引脚，在CH340版上是GND和VU（可以输出5v电压）。  

3、usb供电时v1.0版vin引脚可以5V电压输出，v3版vin引脚没有5V输出,v3版5v电压输出在VU引脚上。  

看网上有说ch340版在烧写时需按flash -> reset。我在操作的时候，没按也是可以烧写的，烧写后也正常工作。  

**新款**

除了上面两款常见版本外，发现某宝上有一款“新款NodeMcu”挺多的,看上面标有 HW-628字样，暂且称他是HW-628。  

有额外32M flash，价格很划算。  在引脚、外观尺寸上完全兼容NodeMcu V1.0 (CP2102)版，有人说信号弱一些，我简单测了一下，不影响使用。而且vin引脚也可以5v电压输出，也就是说，除了外观长的不一样，应该可以替代cp2012版NodeMcu。

下面是三款的合影：

![](https://gitee.com/imgrep001/m1/raw/master/2021/01/21/20210121144425.jpg)





**引脚原理图：**

![](https://gitee.com/imgrep001/m1/raw/master/2021/01/21/20210121143123.png)



  

![](https://gitee.com/imgrep001/m1/raw/master/2021/01/21/20210121143159.png)

### 注意事项

1、NodeMcu的IIC（I2C）管脚是GOIO04和GPIO05，就是上面丝印的D1和D2。