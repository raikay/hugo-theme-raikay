+++
author = "Raikay"  
title = "MySQL数据库SQL优化总结"  
date = "2021-10-25"  
description = "的MySQL数据库SQL优化总结"  
tags = [  
    "mysql",  "sql", 
]  

+++



### 一、避免不走索引的场景
#### 1、尽量避免在字段开头模糊查询  
```sql
-- 放弃索引 全盘扫码
SELECT * FROM t WHERE username LIKE '%陈%'
-- 走索引
SELECT * FROM t WHERE username LIKE '陈%'
-- 优化：如果需求必须前面模糊，用INSTR()优化,虽然也不走索引，不过效率高于like
SELECT * FROM t WHERE INSTR(`username`, '陈');
```
> 数据量较大的情况，建议引用ElasticSearch、solr，亿级数据量检索速度秒级  

#### 2、尽量避免使用in 和not in
```sql
-- 全表扫描
SELECT * FROM t WHERE id IN (2,3)
-- 优化： 如果是连续数值，用between代替
SELECT * FROM t WHERE id BETWEEN 2 AND 3

-- 子查询，可以用exists代替
-- 不走索引
select * from A where A.id in (select id from B);
-- 走索引
select * from A where exists (select * from B where B.id = A.id);
```
#### 3、尽量避免使用 or
```sql
SELECT * FROM t WHERE id = 1 OR id = 3
-- 优化方案: union all 代替
SELECT * FROM t WHERE id = 1
   UNION all
SELECT * FROM t WHERE id = 3
```
#### 4、尽量避免在where条件中等号的左侧进行表达式、函数操作
```sql
-- 全表扫描
SELECT * FROM T WHERE score/10 = 9
-- 走索引
SELECT * FROM T WHERE score = 10*9
```

#### 5、尽量避免进行null值的判断
```sql
SELECT * FROM t WHERE score IS NULL
-- 优化方式：字段加默认值0，对0值进行判断：
SELECT * FROM t WHERE score = 0
```
#### 6、避免使用where 1=1的条件
```sql
SELECT username, age, sex FROM T WHERE 1=1
-- 优化：用代码判断
```

#### 7、查询条件不能用 <> 或者 !=

如确实业务需要，使用到不等于符号，需要在重新评估索引建立，避免在此字段上建立索引，改由查询条件中其他索引字段代替。

#### 8、where条件仅包含复合索引非前置列

如下：复合（联合）索引包含key_part1，key_part2，key_part3三列，但SQL语句没有包含索引前置列"key_part1"，按照MySQL联合索引的最左匹配原则，不会走联合索引。

```sql
select col1 from table where key_part2=1 and key_part3=2
```

#### 9、隐式类型转换造成不使用索引

如下col_varchar类型为varchar，但给定的值为数值，涉及隐式类型转换，造成不能正确走索引。 

```
select col1 from table where col_varchar=123; 
-- 优化 加单引号'123' 匹配正确类型
```

#### 10、order by 条件要与where中条件一致，否则order by不会利用索引进行排序

> group by 、union 、distinct 同理

```
-- 不走age索引
SELECT * FROM t order by age;
 
-- 走age索引
SELECT * FROM t where age > 0 order by age;
```



## 二、SELECT语句其他优化

#### 1、 避免出现select *

会让优化器无法完成索引覆盖扫描这类优化，会影响优化器对执行计划的选择，也会增加网络带宽消耗，更会带来额外的I/O,内存和CPU消耗。


#### 2、 避免使用动态结果的函数

如now()、rand()、sysdate()、current_user()等，造成多库结果不一致、无法利用query cache。


#### 3、多表关联查询时，小表在前，大表在后。

在MySQL中，执行 from 后的表关联查询是从左往右执行的（Oracle相反）。

#### 4、 使用表的别名

当在SQL语句中连接多个表时，请使用表的别名并把别名前缀于每个列名上。这样就可以减少解析的时间并减少哪些友列名歧义引起的语法错误。


#### 5、避免用having替换where

HAVING中的条件一般用于聚合函数的过滤，除此之外，应该将条件写在where字句中。

#### 6、调整Where字句中的连接顺序

MySQL采用从左往右，自上而下的顺序解析where子句。根据这个原理，应将过滤数据多的条件往前放，最快速度缩小结果集。




## 三、增删改 DML 语句优化

## 四、查询条件优化




## 引擎

目前广泛使用的是 MyISAM 和 InnoDB 两种引擎：

#### MyISAM

MyISAM 引擎是 MySQL 5.1 及之前版本的默认引擎，它的特点是：

- 不支持行锁，读取时对需要读到的所有表加锁，写入时则对表加排它锁
- 不支持事务
- 不支持外键
- 不支持崩溃后的安全恢复
- 在表有读取查询的同时，支持往表中插入新纪录
- 支持 BLOB 和 TEXT 的前 500 个字符索引，支持全文索引
- 支持延迟更新索引，极大提升写入性能
- 对于不会进行修改的表，支持压缩表，极大减少磁盘空间占用

#### InnoDB

InnoDB 在 MySQL 5.5 后成为默认索引，它的特点是：

- 支持行锁，采用 MVCC 来支持高并发
- 支持事务
- 支持外键
- 支持崩溃后的安全恢复
- 不支持全文索引（5.6.4之后版本逐渐开始支持）

总体来讲，MyISAM 适合 SELECT 密集型的表，而 InnoDB 适合 INSERT 和 UPDATE 密集型的表


### SQL优化三个原则:

最大化利用索引；  
尽可能避免全表扫描；  
减少无效数据的查询；  



### 数据库引擎  

### 三范式  

### 事务  

### 锁  

### 分页

### 索引  
聚簇索引 非聚簇索引  

### 存储过程  、触发器、事件

### 分库分表分区  

其实，在实际工作中，我们在选择分库分表策略前，想到的应该是从缓存、读写分离、SQL优化等方面，因为这些能够更直接、代价更小的解决问题。
**要记住动表就是动根本，你永远不知道这张表后面会连带多少历史遗留问题**。


### 主从同步  

### 恢复数据 binglog  
binlog

> https://blog.csdn.net/qq_39390545/article/details/106414765



SQL优化
https://blog.csdn.net/qq_39390545/article/details/107020686


#### 1、如果知道查询结果只有一条或者只要最大/最小一条记录，建议用limit 1

