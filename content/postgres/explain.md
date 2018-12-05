---
title: "Explain 执行计划"
date: 2018-12-05T15:27:30+08:00
draft: false
---

##### 文法
```
EXPLAIN [ ( option [, ...] ) ] statement
EXPLAIN [ ANALYZE ] [ VERBOSE ] statement

这里 option可以是：

    ANALYZE [ boolean ]
    VERBOSE [ boolean ]
    COSTS [ boolean ]
    BUFFERS [ boolean ]
    TIMING [ boolean ]
    SUMMARY [ boolean ]
    FORMAT { TEXT | XML | JSON | YAML }
```

##### 注意事项

记住当使用了ANALYZE选项时语句会被实际执行. 如执行dml 时将对数据库进行实际的操作。

避免污染数据的方式

```
BEGIN;
EXPLAIN ANALYZE ...;
ROLLBACK;
```

##### 一个例子

```
postgres=# explain analyze select * from tbl;
                                                    QUERY PLAN                                                    
------------------------------------------------------------------------------------------------------------------
 Seq Scan on tbl  (cost=0.00..144248.48 rows=10000048 width=8) (actual time=0.091..780.145 rows=10000000 loops=1)
 Planning time: 0.124 ms
 Execution time: 1046.394 ms
(3 rows)
```

Seq Scan 顺序扫描  
cost 预估值  
actual 实际值  

[文档 官网](https://www.postgresql.org/docs/10/using-explain.html)  
[文档 中文](http://www.postgres.cn/docs/10/sql-explain.html)
