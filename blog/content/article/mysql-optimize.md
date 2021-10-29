+++
author = "Raikay"  
title = "MySQL数据库SQL优化总结"  
date = "2021-10-25"  
description = "的MySQL数据库SQL优化总结"  
tags = [  
    "mysql",  "sql", 
]  

+++



## 一、避免不走索引的场景
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

#### 7、尽量避免查询条件用 <> 或者 !=

如确实业务需要，使用到不等于符号，让条件列在查询列内，或重新评估索引建立，避免在此字段上建立索引，改由查询条件中其他索引字段代替。

```sql
-- 会使用到覆盖索引
EXPLAIN SELECT `name`, `age`, `pos` FROM `staffs` WHERE `name` != 'Ringo';
-- 索引失效 全表扫描
EXPLAIN SELECT * FROM `staffs` WHERE `name` != 'Ringo';
```

#### 8、where条件仅包含复合索引非前置列

如下：复合（联合）索引包含key_part1，key_part2，key_part3三列，但SQL语句没有包含索引前置列"key_part1"，按照MySQL联合索引的最左匹配原则，不会走联合索引。

```sql
select col1 from table where key_part2=1 and key_part3=2
```

#### 9、避免隐式类型转换

如下col_varchar类型为varchar，但给定的值为数值，涉及隐式类型转换，造成不能正确走索引。 

```sql
select col1 from table where col_varchar=123; 
-- 优化 加单引号'123' 匹配正确类型
```

#### 10、避免order by条件与where条件不一致，否则order by不会利用索引进行排序

> group by 、union 、distinct 同理

```sql
-- 不走age索引
SELECT * FROM t order by age;
-- 走age索引
SELECT * FROM t where age > 0 order by age;
```

### 总结为三个原则:

最大化利用索引；  
尽可能避免全表扫描；  
减少无效数据的查询；  

## 二、查询语句优化

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

#### 6、调整Where中的连接顺序

MySQL采用从左往右，自上而下的顺序解析where子句。根据这个原理，应将过滤数据多的条件往前放，最快速度缩小结果集。

#### 7、巧用limit 1

如果知道查询结果只有一条或者只要最大/最小一条记录，建议用limit 1

#### 8、优化group by语句

默认情况下，MySQL 会对GROUP BY分组的所有值进行排序，如 “GROUP BY col1，col2，....;” 查询的方法如同在查询中指定 “ORDER BY col1，col2，...;” 如果显式包括一个包含相同的列的 ORDER BY子句，MySQL 可以毫不减速地对它进行优化，尽管仍然进行排序。

因此，如果查询包括 GROUP BY 但你并不想对分组的值进行排序，你可以指定 ORDER BY NULL禁止排序。例如：

```sql
SELECT col1, col2, COUNT(*) FROM table GROUP BY col1, col2 ORDER BY NULL ;
```

#### 9、用union all替换union

MySQL通过创建并填充临时表的方式来执行union查询。除非确实要消除重复的行，否则建议使用union all。原因在于如果没有all这个关键词，MySQL会给临时表加上distinct选项，这会导致对整个临时表的数据做唯一性校验，这样做的消耗相当高。  

## 三、建表优化

1、用varchar/nvarchar 代替 char/nchar ，长度只分配真正需要的空间  

2、 在表中建立索引，优先考虑where、order by使用到的字段。   

3、 尽量使用 TINYINT、 SMALLINT、 MEDIUM_INT 作为整数类型而非 INT，如果非负则加上 UNSIGNED  

4、尽量使用 TIMESTAMP 而非 DATETIME  

5、单表不要有太多字段，建议在 20 以内  

6、避免使用 NULL 字段，很难查询优化且占用额外索引空间  

7、字符字段最好不要做主键

8、不用外键，由程序保证约束

## 查询分析

 查询语句钱加EXPLAIN查看是否用了索引

开启慢查询日志分析

```
SET GLOBAL slow_query_log=ON;
```


## 数据库引擎

引擎是数据库的核心服务。目前广泛使用的是 MyISAM 和 InnoDB 两种引擎。

> 还有MEMORY存储引擎，通常很少用到。由于基于内存，所以响应速度非常快。但若内存出现异常就会影响到数据的完整性。若重启机器或者关机，或当mysqld守护进程崩溃时，所有的数据都会丢失。

|                  | InnoDB                              | MyISAM                   |
| ---------------- | ----------------------------------- | ------------------------ |
| 行锁             | 支持，采用 MVCC 来支持高并发        | 不支持                   |
| 事务             | 支持                                | 不支持                   |
| 外键             | 支持                                | 不支持                   |
| 崩溃后的安全恢复 | 支持                                | 不支持                   |
| 全文索引         | 不支持（5.6.4之后版本逐渐开始支持） | 支持                     |
| 索引类型         | 聚集索引                            | 非聚集索引               |
| 默认引擎         | 5.5 后成为默认索引                  | 5.1 及之前版本的默认引擎 |

InnoDB是聚集索引，使用B+Tree作为索引结构，数据文件是和（主键）索引绑在一起的（表数据文件本身就是按B+Tree组织的一个索引结构），必须要有主键，通过主键索引效率很高。但是辅助索引需要两次查询，先查询到主键，然后再通过主键查询到数据。因此，主键不应该过大，因为主键太大，其他索引也都会很大。
MyISAM是非聚集索引，也是使用B+Tree作为索引结构，索引和数据文件是分离的，索引保存的是数据文件的指针。主键索引和辅助索引是独立的。
也就是说：InnoDB的B+树主键索引的叶子节点就是数据文件，辅助索引的叶子节点是主键的值；而MyISAM的B+树主键索引和辅助索引的叶子节点都是数据文件的地址指针  

### 三范式  

第一范式  确保每列保持原子性  
第二范式  确保表中的每列都和主键相关  
第三范式  确保每列都和主键列直接相关,而不是间接相关   

###   事务
MySQL 事务四大特性ACID：原子性、一致性、隔离性、持久性  

### 索引  
索引是存储引擎用于快速找到记录的一种数据结构.  

创建索引:  

```sql
CREATE INDEX index_name ON table_name (column_list)
```

InnoDB使用的是B+树索引.  

### 分库分表分区  

其实，在实际工作中，我们在选择分库分表策略前，想到的应该是从**缓存**、**读写分离**、**SQL优化**等方面，因为这些能够更直接、代价更小的解决问题。
要记住**动表就是动根本**，你永远不知道这张表后面会连带多少历史遗留问题。

### 主从同步 读写分离
主库写从库读，不建议采用双主或多主引入很多复杂性 。  
大致步骤，在主库配置自己的Id，binlog日志位置，从库用来访问主库的账号，在从库配置主库基本信息。    

### 其他知识点
存储过程  、触发器、事件、锁、分页、binglog恢复数据     

**鸣谢**：
> https://blog.csdn.net/qq_39390545/article/details/107020686
> https://blog.csdn.net/qq_39390545/article/details/106414765
> SQL优化最干货总结 - MySQL（2020最新版）
> https://blog.csdn.net/qq_39390545/article/details/107020686
> MySql的数据库引擎
> https://www.cnblogs.com/wangsen/p/10863405.html
> MySql 日常指导，及大表优化思路
> https://juejin.cn/post/6844903663459123208
> mysql主从同步配置
> https://www.cnblogs.com/zhoujie/p/mysql1.html

