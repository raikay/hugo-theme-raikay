+++
author = "Raikay"  
title = "最全的MySQL数据库SQL优化总结"  
date = "2021-10-25"  
description = "最全的MySQL数据库SQL优化总结"  
tags = [  
    "mysql", 
]  

+++



### 一、避免不走索引的场景
#### 1、尽量避免在字段开头模糊查询  
```
#放弃索引 全盘扫码
SELECT * FROM t WHERE username LIKE '%陈%'
#走索引
SELECT * FROM t WHERE username LIKE '陈%'
#必须前面模糊的替代方案,也不走索引，不过效率高于like
SELECT * FROM t WHERE INSTR(`username`, '陈');
```
> 数据量较大的情况，建议引用ElasticSearch、solr，亿级数据量检索速度秒级  
#### 2、尽量避免使用in 和not in
```
#全表扫描
SELECT * FROM t WHERE id IN (2,3)
#连续数值，用between代替
SELECT * FROM t WHERE id BETWEEN 2 AND 3

# 子查询，可以用exists代替
-- 不走索引
select * from A where A.id in (select id from B);
-- 走索引
select * from A where exists (select * from B where B.id = A.id);
```
#### 3、尽量避免使用 or,union all 代替
```
SELECT * FROM t WHERE id = 1 OR id = 3
#优化方案
SELECT * FROM t WHERE id = 1
   UNION all
SELECT * FROM t WHERE id = 3
```

### SQL优化三大原则:

最大化利用索引；  
尽可能避免全表扫描；  
减少无效数据的查询；  