+++
author = "Raikay"  
title = "点亮arduino uno r3板载 / 外接led灯"  
date = "2021-08-25"  
description = "点亮arduino uno r3板载led，亮arduino uno r3外接led灯"  
tags = [  
    "arduino", 
]  

+++

### 1、选择开发板

![IMG](http://blogimg.raikay.com/330617617062694912.png)

### 2、选择端口

![IMG](http://blogimg.raikay.com/330610852275294208.png)

### 3、代码：

```c++
// the setup function runs once when you press reset or power the board
void setup() {
  // initialize digital pin LED_BUILTIN （13） as an output.
  pinMode(LED_BUILTIN, OUTPUT);
}

// the loop function runs over and over again forever
void loop() {
  digitalWrite(LED_BUILTIN, HIGH);   // turn the LED on (HIGH is the voltage level)
  delay(1000);                       // wait for a second
  digitalWrite(LED_BUILTIN, LOW);    // turn the LED off by making the voltage LOW
  delay(1000);                       // wait for a second
}
```

### 4、点击上传代码，上传后led开始闪烁。

![IMG](http://blogimg.raikay.com/330618074669649920.png)

### 点亮外接LED灯

```c++
int LED = 7;

void setup() {               
  // 定义7为输出引脚
  pinMode(LED, OUTPUT);     
}

void loop() {
  digitalWrite(LED, HIGH);   // 点亮LED
  delay(1000);               // 持续1秒
  digitalWrite(LED, LOW);    // 熄灭LED
  delay(1000);               // 持续1秒
}
	
```
![IMG](http://blogimg.raikay.com/330611027626561536.jpg)