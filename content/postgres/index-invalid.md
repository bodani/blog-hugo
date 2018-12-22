---
title: "引起索引失效"
date: 2018-12-20T16:34:22+08:00
draft: false
---

如果where过滤条件设置不合理，即使索引存在，且where过滤条件中包含索引列，也会导致全表扫描，索引不起作用。什么条件下会导致索引失效呢？

1.任何计算、函数、类型转换

2.!=

3.NOT，相当于使用函数

4.模糊查询通配符在开头

5.索引字段在表中占比较高

6.多字段btree索引查询条件不包含第一列

7.多字段索引查询条件使用OR（有时也会走索引扫描，但查询效率不高）

索引在哪些情况下失效
https://www.cnblogs.com/alianbog/p/5648455.html
https://blog.csdn.net/u012150590/article/details/79943541
https://blog.csdn.net/u010793461/article/details/79851372
https://www.cnblogs.com/areyouready/p/7802885.html

关于 null 
https://yq.aliyun.com/articles/241219

索引失效原因
https://blog.csdn.net/colin_liu2009/article/details/7301089

查看表顺序扫描和索引的次数
select * from pg_stat_all_tables where relname = 'tab_name';

创建索引
http://www.cnblogs.com/alianbog/p/5631505.html 
