+++
author = "Raikay"  
title = "HTTP / HTTPS 必备基础知识"  
date = "2020-12-05"  
description = "http / https 必备基础知识"  
tags = [
    "http",
]  

+++



![img](https://gitee.com/imgrep001/m1/raw/master/20200811132811.jpeg)



HTTP 是一种 `超文本传输协议(Hypertext Transfer Protocol)`。  

http是一个简单的请求-响应协议，它通常运行在TCP之上。它指定了客户端可能发送给服务器什么样的消息以及得到什么样的响应 。  



## 1、地址栏输入网址（url）后发生了什么

**1. DNS解析**

首先浏览器通过DNS解析根据域名获取IP。通过下面的顺序获取。其中某个节点返回IP就不继续执行。  

> 浏览器缓存 --> 系统host --> 本地域名服务器LDNS --> Root Server 域名服务器  

**2. 建立连接**

浏览器根据得到的IP和目标服务器经过三次握手建立 TCP 连接。  

**3. 请求**

建立连接后，浏览器会向目标服务器发起 `HTTP-GET` 请求，包括其中的 URL，HTTP 1.1 后默认使用长连接，只需要一次握手即可多次传输数据。

**4. 响应**

如果目标服务器只是一个简单的页面，就会直接返回，状态码为200。如果有重定向 ，返回 301,302 以 3 开头的重定向码，浏览器在获取了重定向响应后，在响应报文中 Location  项找到重定向地址，浏览器重新第一步访问即可。

**5. 关闭连接**

应答结束后，web浏览器与webserver四次握手断开连接，以保证其它web浏览器能够与webserver建立连接。

http1.1不是立即断开连接，而是服务端使用超时断开的策略来维护连接，一般是300s左右

## 2、无状态协议

HTTP无状态协议，是指协议对于事务处理没有记忆能力。每次的请求都是独立的，它的执行情况和结果与前面的请求和之后的请求是无直接关系的。不过，可以通过cookie、session等机制解决这个问题。

![](https://gitee.com/imgrep001/m1/raw/master/20200811132905.png)



## 3、HTTP请求方法

根据 HTTP 标准，HTTP 请求可以使用多种请求方法。  
HTTP1.0 定义了三种请求方法： GET, POST 和 HEAD方法。  
HTTP1.1 新增了六种请求方法：OPTIONS、PUT、PATCH、DELETE、TRACE 和 CONNECT 方法。  

| 方法    | 描述                                                         |
| ------- | ------------------------------------------------------------ |
| GET     | 请求指定的页面信息，并返回实体主体。                         |
| POST    | 向指定资源提交数据进行处理请求（例如提交表单或者上传文件）。数据被包含在请求体中。POST 请求可能会导致新的资源的建立和/或已有资源的修改。 |
| PUT     | 从客户端向服务器传送的数据取代指定的文档的内容。             |
| DELETE  | 请求服务器删除指定的页面。                                   |
| HEAD    | 类似于 GET 请求，只不过返回的响应中没有具体的内容，用于获取报头 |
| CONNECT | 协议中预留给能够将连接改为管道方式的代理服务器。             |
| OPTIONS | 允许客户端查看服务器的性能。                                 |
| TRACE   | 回显服务器收到的请求，主要用于测试或诊断。                   |
| PATCH   | 对 PUT 方法的补充，用来对已知资源进行局部更新 。             |

**rest full api**

GET /device-management/devices ：获取所有设备  
POST /device-management/devices：创建新设备    （是幂等的。多次发送重试请求，等同于单个请求修改。）  

GET /device-management/devices/{id} ：获取设备信息由“id”  
PUT /device-management/devices/{id} ：更新由“id”标识的设备信息  （不是幂等的。重试请求N次，最终拥有N个资源）  
DELETE /device-management/devices/{id} ：通过“id”删除设备    

## 4、HTTP报文

在HTTP连接中报文分为请求（request）和响应（response）两种。每种报文在请求头都有不同的字段来标识不同的用途。

![](https://gitee.com/imgrep001/m1/raw/master/20200811133028.png)

**常见请求头：**

`Accept`：浏览器可接受的MIME类型；

`Accept-Charset`：浏览器可接受的字符集；

`Accept-Encoding`：浏览器能够进行解码的数据编码方式，比如gzip。

`Accept-Language`：浏览器所希望的语言种类，当服务器能够提供一种以上的语言版本时要用到；

`Authorization`：授权信息，通常出现在对服务器发送的WWW-Authenticate头的应答中；

`Connection`：表示是否需要持久连接。

`Content-Length`：表示请求消息正文的长度；

`Cookie`：这是最重要的请求头信息之一；

`From`：请求发送者的email地址，由一些特殊的Web客户程序使用，浏览器不会用到它；

`Host`：初始URL中的主机和端口；

If-Modified-Since：只有当所请求的内容在指定的日期之后又经过修改才返回它，否则返回304“Not Modified”应答；

`Pragma`：指定“no-cache”值表示服务器必须返回一个刷新后的文档，即使它是代理服务器而且已经有了页面的本地拷贝；

`Referer`：包含一个URL，用户从该URL代表的页面出发访问当前请求的页面。

`User-Agent`：浏览器类型，如果Servlet返回的内容与浏览器类型有关则该值非常有用；

`UA-Pixels，UA-Color，UA-OS，UA-CPU`：由某些版本的IE浏览器所发送的非标准的请求头，表示屏幕大小、颜色深度、操作系统和CPU类型。

**常见响应头：**

`Allow`：服务器支持哪些请求方法（如GET、POST等）；

`Content-Encoding`：文档的编码（Encode）方法。只有在解码之后才可以得到Content-Type头指定的内容类型。利用gzip压缩文档能够显著地减少HTML文档的下载时间。

`Content-Length`：表示内容长度。只有当浏览器使用持久HTTP连接时才需要这个数据。

`Content-Type`： 表示后面的文档属于什么MIME类型。

`Date`：当前的GMT时间。

`Expires`：指明应该在什么时候认为文档已经过期，从而不再缓存它。

`Last-Modified`：文档的最后改动时间。客户可以通过If-Modified-Since请求头提供一个日期，该请求将被视为一个条件GET，只有改动时间迟于指定时间的文档才会返回，否则返回一个304（Not Modified）状态。Last-Modified也可用setDateHeader方法来设置；

`Location`：表示客户应当到哪里去提取文档。Location通常不是直接设置的，而是通过HttpServletResponse的sendRedirect方法，该方法同时设置状态代码为302；

`Refresh`：表示浏览器应该在多少时间之后刷新文档，以秒计。



## 5、TCP 三次握手和四次挥手

### 三次握手

**相关概念**：

**SYN**：它的全称是 `Synchronize Sequence Numbers`，同步序列编号。是 TCP/IP 建立连接时使用的握手信号。在客户机和服务器之间建立 TCP 连接时，首先会发送的一个信号。客户端在接受到 SYN 消息时，就会在自己的段内生成一个随机值 X。    

**SYN-ACK**：服务器收到 SYN 后，打开客户端连接，发送一个 SYN-ACK 作为答复。确认号设置为比接收到的序列号多一个，即 X + 1，服务器为数据包选择的序列号是另一个随机数 Y。    

**ACK**：`Acknowledge character`, 确认字符，表示发来的数据已确认接收无误。最后，客户端将 ACK 发送给服务器。序列号被设置为所接收的确认值即 Y + 1。    

**FIN**：用来释放一个连接。FIN=1表示：此报文段的发送方的数据已经发送完毕，并要求释放运输连接

如果用现实生活来举例的话就是  

小明 - 客户端 小红 - 服务端

- 小明给小红打电话，接通了后，小明说喂，能听到吗，这就相当于是连接建立。

- 小红给小明回应，能听到，你能听到我说的话吗，这就相当于是请求响应。

- 小明听到小红的回应后，好的，这相当于是连接确认。在这之后小明和小红就可以通话/交换信息了。

![](https://gitee.com/imgrep001/m1/raw/master/20200811133110.png)



### 四次挥手

在连接终止阶段使用四次挥手，连接的每一端都会独立的终止。下面我们来描述一下这个过程。

- 首先，客户端应用程序决定要终止连接(这里服务端也可以选择断开连接)。这会使客户端将 FIN 发送到服务器，并进入 `FIN_WAIT_1` 状态。当客户端处于 FIN_WAIT_1 状态时，它会等待来自服务器的 ACK 响应。
- 然后第二步，当服务器收到 FIN 消息时，服务器会立刻向客户端发送 ACK 确认消息。
- 当客户端收到服务器发送的 ACK 响应后，客户端就进入 `FIN_WAIT_2` 状态，然后等待来自服务器的 `FIN` 消息
- 服务器发送 ACK 确认消息后，一段时间（可以进行关闭后）会发送 FIN 消息给客户端，告知客户端可以进行关闭。
- 当客户端收到从服务端发送的 FIN 消息时，客户端就会由 FIN_WAIT_2 状态变为 `TIME_WAIT` 状态。处于 TIME_WAIT 状态的客户端允许重新发送 ACK 到服务器为了防止信息丢失。客户端在 TIME_WAIT 状态下花费的时间取决于它的实现，在等待一段时间后，连接关闭，客户端上所有的资源（包括端口号和缓冲区数据）都被释放。



还是可以用上面那个通话的例子来进行描述

- 小明对小红说：“我要挂电话了”。
- 小红说：“知道了，等我把话说完”。
- 经过若干秒后，小红也说完了，小红说：“说完了，可以挂了”。
- 小明收到消息后，又等了若干时间后，挂断了电话。

![](https://gitee.com/imgrep001/m1/raw/master/20200811133151.png)





## 6、简述 HTTP1.0/1.1/2.0 的区别


### HTTP 1.0

HTTP 1.0 是在 1996 年引入的，从那时开始，它的普及率就达到了惊人的效果。

- HTTP 1.0 仅仅提供了最基本的认证，这时候用户名和密码还未经加密，因此很容易收到窥探。
- HTTP 1.0 被设计用来使用短链接，即每次发送数据都会经过 TCP 的三次握手和四次挥手，效率比较低。
- HTTP 1.0 只使用 header 中的 If-Modified-Since 和 Expires 作为缓存失效的标准。
- HTTP 1.0 不支持断点续传，也就是说，每次都会传送全部的页面和数据。
- HTTP 1.0 认为每台计算机只能绑定一个 IP，所以请求消息中的 URL 并没有传递主机名（hostname）。

### HTTP 1.1

HTTP 1.1 是 HTTP 1.0 开发三年后出现的，也就是 1999 年，它做出了以下方面的变化

- HTTP 1.1 使用了摘要算法来进行身份验证
- HTTP 1.1 默认使用长连接，长连接就是只需一次建立就可以传输多次数据，传输完成后，只需要一次切断连接即可。长连接的连接时长可以通过请求头中的 `keep-alive` 来设置
- HTTP 1.1 中新增加了 E-tag，If-Unmodified-Since, If-Match, If-None-Match 等缓存控制标头来控制缓存失效。
- HTTP 1.1 支持断点续传，通过使用请求头中的 `Range` 来实现。
- HTTP 1.1 使用了虚拟网络，在一台物理服务器上可以存在多个虚拟主机（Multi-homed Web Servers），并且它们共享一个IP地址。

### HTTP 2.0

HTTP 2.0 是 2015 年开发出来的标准，它主要做的改变如下

- `头部压缩`，由于 HTTP 1.1 经常会出现 **User-Agent、Cookie、Accept、Server、Range** 等字段可能会占用几百甚至几千字节，而 Body 却经常只有几十字节，所以导致头部偏重。HTTP 2.0 使用 `HPACK` 算法进行压缩。
- `二进制格式`，HTTP 2.0 使用了更加靠近 TCP/IP 的二进制格式，而抛弃了 ASCII 码，提升了解析效率
- `强化安全`，由于安全已经成为重中之重，所以 HTTP2.0 一般都跑在 HTTPS 上。
- `多路复用`，即每一个请求都是是用作连接共享。一个请求对应一个id，这样一个连接上可以有多个请求。

![img](https://gitee.com/imgrep001/m1/raw/master/20200811133239.png)

### HTTP 简单总结

- HTTP/1.x 有连接无法复用、队头阻塞、协议开销大和安全因素等多个缺陷；

- HTTP/2 通过多路复用、二进制流、Header 压缩等等技术，极大地提高了性能，但是还是存在着问题的；

- http3基于utp 协议的QUIC协议，效率更好，丢失包也不会导致包阻塞，而且它采用64位uid 作为标识，不会导致我们切换网络时候因为ip的变化导致传输中断。

  

## 7、HTTPS 
HTTPS 的全称是 `Hypertext Transfer Protocol Secure`，从名称我们可以看出 HTTPS 要比 HTTPS 多了 secure 安全性这个概念，实际上， HTTPS 并不是一个新的应用层协议，它其实就是 HTTP + TLS/SSL 协议组合而成，而安全性的保证正是 TLS/SSL 所做的工作。

也就是说，**HTTPS 就是身披了一层 SSL 的 HTTP**。

## 1、uri、url 区别

URI（Uniform Resource Identifier）：统一资源标识符，一个资源的唯一标识，可以是一个名字、一串编号、一个URL（说明URL是一种URI）。

URL（Uniform Resource Locator）：统一资源定位符，一个资源的网络地址

**两者关系**

1. URI负责识别，URL负责定位

2. URL是URI的子集（URL一定是URI，但URI不一定是URL）

3. URI是一个唯一字符串，URL是一个表示位置（或地址）的唯一字符串

**举例说明**

1. 我要找到这个骚扰如花的油腻大叔！谁能提供他的信息？
2. 普通URI的告知方式：他的身份证号、纹图案、DNA检测报告
3. URL（一种特殊的URI）的告知方式：动物住址协议://地球/中国/广东省/深圳市/南山区/XX小区/X栋X座24楼/5号座位/陈狗蛋

**引申领域**

URI还有其他种类，这些平时接触不多。

URN（Uniform Resource Name）：统一资源名称

URC（Uniform Resource Characteristics）：统一资源特性

Data URI（Data URI scheme）

![img](https://gitee.com/imgrep001/m1/raw/master/20200811133323.png)


**鸣谢：**
```xml
https://www.zhihu.com/question/21950864
https://www.cnblogs.com/hustskyking/p/data-uri.html
https://www.cnblogs.com/luyuqiang/p/uri-url-urn-urc-and-data-uri.html
https://mp.weixin.qq.com/s/d9mDXO-dxeYUPMIii_scrg
https://www.cnblogs.com/liguangsunls/p/7248357.html
https://blog.csdn.net/dreamingbaobei3/article/details/95938517
https://www.cnblogs.com/wxf-h/p/10519670.html
https://www.cnblogs.com/laoluoits/
#阮一峰 RESTful API 设计指南
http://www.ruanyifeng.com/blog/2014/05/restful_api.html
```