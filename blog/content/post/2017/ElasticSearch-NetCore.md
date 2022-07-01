+++
author = "Raikay"
title = "ElasticSearch入门 附.NET Core例子 "
date = "2017-03-11"
description = "ElasticSearch入门 附.NET Core例子：是一个支持分布式的全文搜索引擎，因为在海量数据搜索时，普通关系型、非关系型数据库因为IO读取、处理器运算能力的限制，导致查询效率难以提升，但是ES是分布式的（能把处理压力分摊给每个节点），而且它是给每个词创建索引，所以查询效率极高，堪称即时搜索。"
tags = [
    "ElasticSearch",
]

+++

​            

## 1.什么是ElasticSearch?

Elasticsearch是基于Lucene的搜索引擎。它提供了一个分布式，支持多租户的全文搜索引擎，它具有HTTP Web界面和无模式JSON文档。 Elasticsearch是用Java开发的，根据Apache许可条款作为开源发布。

*----来自维基百科的解释*

我个人的理解是Elasticsearch（以下简称ES）是一个支持分布式的全文搜索引擎，因为在海量数据搜索时，普通关系型、非关系型数据库因为IO读取、处理器运算能力的限制，导致查询效率难以提升，但是ES是分布式的（能把处理压力分摊给每个节点），而且它是给每个词创建索引，所以查询效率极高，堪称即时搜索。

而且ES能搭配Kibana,实现数据的可视化管理与数据分析。

## 2.ES中名词概念

### 2.1 Node和Cluster

前面所过ES是一个分布式搜索引擎，其本质是一个分布式数据库，可以多台计算机上的ES实例协同工作，这里面的某一台计算机上的某个ES实例，就可以称为一个Node(节点)，所有的这些协同工作的实例，可以称为一个Cluster(集群)。

### 2.2 Index

Elastic 会索引所有字段，经过处理后写入一个反向索引（Inverted Index）。查找数据的时候，直接查找该索引。

所以，Elastic 数据管理的顶层单位就叫做 Index（索引）。它是单个数据库的同义词。每个 Index （即数据库）的名字必须是小写。

### 2.3 Document

Index 里面单条的记录称为 Document（文档）。许多条 Document 构成了一个 Index。

Document 使用 JSON 格式表示，下面是一个例子。

```json
{
  "user": "张三",
  "title": "工程师",
  "desc": "数据库管理"
}
```

同一个 Index 里面的 Document，不要求有相同的结构（scheme），但是最好保持相同，这样有利于提高搜索效率。

### 2.4 Type

Document 可以分组，比如`weather`这个 Index 里面，可以按城市分组（北京和上海），也可以按气候分组（晴天和雨天）。这种分组就叫做 Type，它是虚拟的逻辑分组，用来过滤 Document。

不同的 Type 应该有相似的结构（schema），举例来说，`id`字段不能在这个组是字符串，在另一个组是数值。这是与关系型数据库的表的[一个区别](https://www.elastic.co/guide/en/elasticsearch/guide/current/mapping.html)。性质完全不同的数据（比如`products`和`logs`）应该存成两个 Index，而不是一个 Index 里面的两个 Type（虽然可以做到）。

根据[规划](https://www.elastic.co/blog/index-type-parent-child-join-now-future-in-elasticsearch)，Elastic 6.x 版只允许每个 Index 包含一个 Type，7.x 版将会彻底移除 Type。

*----参考*[*阮一峰 全文搜索引擎 Elasticsearch 入门教程*](http://www.ruanyifeng.com/blog/2017/08/elasticsearch.html)

### 3.ES工作原理

Elasticsearch用于构建高可用和可扩展的系统。扩展的方式可以是购买更好的服务器(纵向扩展)或者购买更多的服务器（横向扩展）。

Elasticsearch虽然能从更强大的硬件中获得更好的性能，但是纵向扩展有它的局限性。真正的扩展应该是横向的，它通过增加节点来均摊负载和增加可靠性。如果我们启动一个单独的节点，它还没有数据和索引，这个集群看起来就像下图。

![img](http://blogimg.raikay.com/303689104409890816.png)

集群中一个节点会被选举为主节点(master),它将临时管理集群级别的一些变更，例如新建或删除索引、增加或移除节点等。

主节点不参与文档级别的变更或搜索，这意味着在流量增长的时候，该主节点不会成为集群的瓶颈。任何节点都可以成为主节点。我们例子中的集群只有一个节点，所以它会充当主节点的角色。

当索引创建完成的时候，主分片的数量就固定了，但是复制分片的数量可以随时调整。

让我们在集群中唯一一个空节点上创建一个叫做 blogs 的索引。默认情况下，一个索引被分配5个主分片，但是为了演示的目的，我们只分配3个主分片和一个复制分片（每个主分片都有一个复制分片）：

PUT /blogs

```json
{
    "settings":{
        "number_of_shards":3,
        "number_of_replicas":1
    }
}
```

![img](http://blogimg.raikay.com/303689431976644608.png)

我们的集群现在看起来就像上图，三个主分片都被分配到 Node 1 。

在单一节点上运行意味着有单点故障的风险：没有数据备份。幸运的是，要防止单点故障，我们唯一需要做的就是启动另一个节点。

如果我们启动了第二个节点，这个集群看起来就像下图

![img](http://blogimg.raikay.com/303689745224044544.png)

第二个节点已经加入集群，三个复制分片(replica shards)也已经被分配了，分别对应三个主分片，这意味着在丢失任意一个节点的情况下依旧可以保证数据的完整性。

文档的索引将首先被存储在主分片中，然后并发复制到对应的复制节点上。这可以确保我们的数据在主节点和复制节点上都可以被检索。

随着应用需求的增长，我们该如何扩展？如果我们启动第三个节点，我们的集群会自我感知，这时便成为了三节点集群。

分片已经被重新分配以平衡负载：

![img](http://blogimg.raikay.com/303689765973266432.png)

从 Node 1 和 Node 2 来的分片已经被移动到新的 Node 3 上，这样每个节点就有两个分片，以代替之前的三个。这意味着每个节点的硬件资源（CPU、RAM、I/O）被较少的分片共享，这样每个分片就会有更好的表现。

分片本身就是一个完整成熟的搜索引擎，它可以使用单一节点的所有资源。使用这6个分片（3个主分片和三个复制分片）我们可以扩展最多到6个节点，每个节点上有一个分片，这样就可以100%使用这个节点的资源了。

*----参考文献*[*Elasticsearch: 权威指南*](https://www.elastic.co/guide/cn/elasticsearch/guide/current/index.html)

## 4.ES的安装与使用

### 4.1安装

因为每个平台上ES安装方法有所区别，而且网络上有较为详细的安装教程，本文在此不再赘述。原本是想着在我的两台腾讯云Centos服务器上，搭建一个ES集群的，但是因为云服务器内存1G，运行ES时总是报错，大体意思是内存不足，所以我就在自己的PC上,只搭建了一个ES节点，还不算ES集群，不过不影响功能的测试。

Windows环境下ES 6.4 MSI下载地址：https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-6.4.0.msi

一路默认下一步，安装完成后，在浏览器地址输入http://localhost:9200/，如果您能看到下列结果，说明安装完成。

```json
{
    "name": "DESKTOP-1FC1B1D",
    "cluster_name": "elasticsearch",
    "cluster_uuid": "lZx4n2xzToeaj9k3HEHAqw",
    "version": {
        "number": "6.4.0",
        "build_flavor": "unknown",
        "build_type": "unknown",
        "build_hash": "595516e",
        "build_date": "2018-08-17T23:18:47.308994Z",
        "build_snapshot": false,
        "lucene_version": "7.4.0",
        "minimum_wire_compatibility_version": "5.6.0",
        "minimum_index_compatibility_version": "5.0.0"
    },
    "tagline": "You Know, for Search"
}
```

"You Know, for Search"

ES有一套Restful 风格的API系统，通过该API我们与ES进行交互。

### 4.2数据的提交

利用PostMan向ES POST一条数据如下。

![image](http://blogimg.raikay.com/303689786080759808.png)

http://localhost:9200/index/test1/1 中Index是该数据的Index(上文有介绍Index),test1是该数据的Type,1是该条数据的Id,该ID在通过ID获取数据时需要用到。

### 4.3数据通过ID获取

在知道数据的Index,Type和ID的情况下，可以通过和上文Post数据的Url一样的格式获取数据，不同之处时，此时的HTTP方法时Get,如下：

![image](http://blogimg.raikay.com/303689934894665728.png)

### 4.4数据的查询

ES的数据查询语法较为丰富，此处以一个最简单的查询为例，Http方法为POST,请求的Url中同样指定了Index和Type

```json
{
    "query":{
        "match":{
            "tagline":"for"
        }
    }
}
```



指的时查询tagline中包含的for的数据，

![image](http://blogimg.raikay.com/303689963592093696.png)

其他更详细的查询语法，建议大家查看[*Elasticsearch: 权威指南*](https://www.elastic.co/guide/cn/elasticsearch/guide/current/index.html)，此处主要抛砖引玉。

## 5.Net Core中使用ES

在上文中，我们了解到，可以通过restful api与ES进行交互，那么，如果需要在我们的程序中使用ES，是不是要创建一个这样的Helper方法，通过HTTP调用RESTFul API与ES进行交互呢？

不是不可以，但是Elastic为大部分语言都创建了"Clients”，其实就是把上文提及的那些方法进行了一个封装，是我们在代码中，能够方便地调用ES。

以.Net Core为例，该”Clients”开源在Github:

https://github.com/elastic/elasticsearch-net

### 5.1 SDK（客户端,Clients）

在该仓库中，其实有`Elasticsearch.Net` 和 `NEST`两个.Net官方SDK,两个各有特色。

`Elasticsearch.Net` 是一个非常low  leave而且灵活的SDK，它不在意你如何的构建自己的请求和响应。它非常抽象，因此所有的Elasticsearch RESTFul  API被表示为方法，而且不会影响你构建json / request / response对象的方式。  它还内置可配置/可覆盖的群集故障转移重试机制。

`NEST` 是一个 high level SDK,  有非常大的弹性，如果你想更好的提升你的搜索服务，你完全可以使用它来做为你的客户端。可以映射所有请求和响应对象，拥有一个强类型DSL（领域特定语言），并且可以使用.net的特性，如协变、Auto Mapping Of POCOs，NEST内部使用的依然是Elasticsearch.Net客户端。

### 5.2创建一个Demo

本Demo我使用的NEST，所以第一步是创建一个Asp.Net Core Api应用程序并引入NEST的Nuget包。

```sh
PM> Install-Package NEST
```

然后我创建一个EsClientProvider,代码如下：



```
public class EsClientProvider : IEsClientProvider
    {
        private readonly IConfiguration _configuration;
        private ElasticClient _client;
        public EsClientProvider(IConfiguration configuration)
        {
            _configuration = configuration;
        }

        public ElasticClient GetClient()
        {
            if (_client != null)
                return _client;

            InitClient();
            return _client;
        }

        private void InitClient()
        {
            var node = new Uri(_configuration["EsUrl"]);
            _client = new ElasticClient(new ConnectionSettings(node).DefaultIndex("demo"));
        }
    }
```

IEsClientProvider代码如下：

```
public interface IEsClientProvider
    {
        ElasticClient GetClient();
    }
```

然后再Startup的ConfigureServices进行服务的注册

```
public void ConfigureServices(IServiceCollection services)
        {

            services.AddSingleton<IEsClientProvider, EsClientProvider>();

            services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);
        }
```

最后，修改ValueContoller,代码如下：

```
public class ValueController : ControllerBase
    {
        private readonly ElasticClient _client;

        public ValueController(IEsClientProvider clientProvider)
        {
            _client = clientProvider.GetClient();
        }

        [HttpPost]
        [Route("value/index")]
        public IIndexResponse Index(Post post)
        {
            return _client.IndexDocument(post);
        }

        [HttpPost]
        [Route("value/search")]
        public IReadOnlyCollection<Post> Search(string type)
        {
            return _client.Search<Post>(s => s
                .From(0)
                .Size(10)
                .Query(q => q.Match(m => m.Field(f => f.Type).Query(type)))).Documents;
        }
    }
```



其中Index方法能进行数据的提交，Search是通过Post实体的type来进行数据查询。  

代码不复杂，我就不详细介绍了，在PostMan中进行Search方法的测试，效果如下：  

![image](http://blogimg.raikay.com/303690029811765248.png)

查询要求是type是567,响应的实体中，type确实为567,Success!  

项目完整代码：https://github.com/liuzhenyulive/Elasticsearch.Net-Demo

鸣谢：

> 原文：https://www.cnblogs.com/CoderAyu/p/9601991.html