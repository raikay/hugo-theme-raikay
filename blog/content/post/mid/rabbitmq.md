+++
author = "Raikay"
title = "如何用好消息队列RabbitMQ？ "
date = "2017-03-11"
description = "如何用好消息队列RabbitMQ？RabbitMQ 是基于 AMQP 实现的一个开源消息组件，主要用于在分布式系统中存储转发消息，由因高性能、高可用以及高扩展而出名的 Erlang 语言写成 "
tags = [
    "数据库",
]

+++



### 一、RabbitMQ 简介 

RabbitMQ 是基于 AMQP 实现的一个开源消息组件，主要用于在分布式系统中存储转发消息，由因高性能、高可用以及高扩展而出名的 Erlang 语言写成。

其中，AMQP（Advanced Message Queuing Protocol，即高级消息队列协议），是一个异步消息传递所使用的应用层协议规范，为面向消息的中间件设计。

![img](https://raikay.coding.net/p/code/d/m1/git/raw/master/20200811135727.png)

### 二、为什么要用消息队列 MQ 

- 1、业务系统往往要求响应能力特别强，能够起到削峰填谷的作用。

- 2、解耦：如果一个系统挂了，则不会影响另外个系统的继续运行。

- 3、业务系统往往有对消息的高可靠要求，以及有对复杂功能如 Ack 的要求。

- 4、增强业务系统的异步处理能力，减少甚至几乎不可能出现并发现象：

使用消息队列，就好比为了防汛而建葛洲坝，有大量数据的堆积能力，然后可靠地进行异步输出。例如：

![img](https://raikay.coding.net/p/code/d/m1/git/raw/master/20200811135626.png)

传统做法存在如下问题，请见上图：

- 1、一旦业务处理时间超过了定时器时间间隔，就会导致漏单。

- 2、如果采用新开线程的方式获取数据，那么由于大量新开线程处理，会容易造成服务器宕机。

- 3、数据库压力大，易并发。

![img](https://raikay.coding.net/p/code/d/m1/git/raw/master/20200811135548.png)

**使用 MQ 后的好处，请见上图**：

- 1、业务可注册、可配置。

- 2、获取数据规则可配置。

- 3、成功消费 MQ 中的消息才会被 Ack，提高可靠性。

- 4、大大增强了异步处理业务作业的能力：

定时从数据库获取数据后，存入 MQ 消息队列，然后 Job 会定期扫描 MQ 消息队列，假设 Job 扫描后先预取 5 条消息，然后异步处理这 5 条消息，也就是说这 5 条消息可能会同时被处理。



#### RabbitMQ 特点如下：

**高可靠**：RabbitMQ 提供了多种多样的特性让你在可靠性和性能之间做出权衡，包括持久化、发送应答、发布确认以及高可用性。

**高可用队列**：支持跨机器集群，支持队列安全镜像备份，消息的生产者与消费者不论哪一方出现问题，均不会影响消息的正常发出与接收。

**灵活的路由**：所有的消息都会通过路由器转发到各个消息队列中，RabbitMQ 内建了几个常用的路由器，并且可以通过路由器的组合以及自定义路由器插件来完成复杂的路由功能。

**支持多客户端**：对主流开发语言（如：Python、Ruby、.NET、Java、C、PHP、ActionScript 等）都有客户端实现。

**集群**：本地网络内的多个 Server 可以聚合在一起，共同组成一个逻辑上的 broker。

**扩展性**：支持负载均衡，动态增减服务器简单方便。

**权限管理**：灵活的用户角色权限管理，Virtual Host 是权限控制的最小粒度。

**插件系统**：支持各种丰富的插件扩展，同时也支持自定义插件，其中最常用的插件是 Web 管理工具 RabbitMQ_Management，其 Web UI 访问地址：



### 三、RabbitMQ 工作原理 

消息从发送端到接收端的流转过程即 RabbitMQ 的消息工作机制，请见下图：

![img](https://raikay.coding.net/p/code/d/m1/git/raw/master/20200811135511.png)

消息发送与接收的工作机制

### 四、RabbitMQ 基本用法 

共有 6 种基本用法：单对单、单对多、发布订阅模式、按路由规则发送接收、主题、RPC（即远程存储调用）。我们将介绍单对单、单对多和主题的用法。

1、**单对单**：单发送、单接收。请见下图。

![img](https://raikay.coding.net/p/code/d/m1/git/raw/master/20200811135413.png)

2、**单对多**：一个发送端，多个接收端，如分布式的任务派发。请见下图：

![img](https://raikay.coding.net/p/code/d/m1/git/raw/master/20200811135329.png)

3、**主题**：Exchange Type 为 topic，发送消息时需要指定交换机及 Routing Key，消费者的消息队列绑定到该交换机并匹配到 Routing Key 实现消息的订阅，订阅后则可接收消息。只有消费者将队列绑定到该交换机且指定的 Routing Key 符合匹配规则，才能收到消息。

其中 Routing Key 可以设置成通配符，如：*或 #（*表示匹配 Routing Key 中的某个单词，# 表示任意的 Routing Key 的消息都能被收到）。如果 Routing Key 由多个单词组成，则单词之间用. 来分隔。

![img](https://raikay.coding.net/p/code/d/m1/git/raw/master/20200811135221.png)



**命名规范**：

交换机名的命名建议：Ex{AppID}.{自定义 ExchangeName}，队列名的命名建议：MQ{AppID}.{自定义 QueueName} 。

### 五、Demo 下载及更多资料 

**RabbitMQDemo 下载地址**：

https://github.com/das2017/RabbitMQDemo

**RabbitMQ 的官方网址**：

http://www.rabbitmq.com

鸣谢：
> 原作者: 杨丽、张辉清  
> 原作者公众号: 聊聊架构
