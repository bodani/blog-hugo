---
title: "pgstattuple 表空间膨胀"
date: 2019-05-22T17:26:45+08:00
draft: false
---

#### 背景介绍

监控表空间膨胀

pgstattuple提供了pgstatetuple()和pgstatindex()两个统计表和索引的方法，较PostgreSQL系统表pg_class的表统计信息，pgstatetuple()还统计了表中的dead tuples。

https://www.postgresql.org/docs/current/pgstattuple.html
#### 简单使用

```
test=# create  table tb3(id integer,name character varying);
CREATE TABLE
test=# \d+ tb3 
                                  数据表 "public.tb3"
 栏位 |       类型        | Collation | Nullable | Default |   存储   | 统计目标 | 描述 
------+-------------------+-----------+----------+---------+----------+----------+------
 id   | integer           |           |          |         | plain    |          | 
 name | character varying |           |          |         | extended |          | 

test=#  alter table tb3 add primary key(id);
ALTER TABLE
test=# \d+ tb3 
                                  数据表 "public.tb3"
 栏位 |       类型        | Collation | Nullable | Default |   存储   | 统计目标 | 描述 
------+-------------------+-----------+----------+---------+----------+----------+------
 id   | integer           |           | not null |         | plain    |          | 
 name | character varying |           |          |         | extended |          | 
索引：
    "tb3_pkey" PRIMARY KEY, btree (id)

test=# insert into tb3 select generate_series(1,1000000),md5(random()::text);
INSERT 0 1000000

test=# create extension pgstattuple ;
CREATE EXTENSION
test=# select * from pgstattuple('tb3');
-[ RECORD 1 ]------+---------
table_len          | 68272128
tuple_count        | 1000000
tuple_len          | 61000000
tuple_percent      | 89.35
dead_tuple_count   | 0
dead_tuple_len     | 0
dead_tuple_percent | 0
free_space         | 38776
free_percent       | 0.06

test=# select * from pgstatindex('tb3_pkey');
-[ RECORD 1 ]------+---------
version            | 2
tree_level         | 2
index_size         | 22487040
root_block_no      | 412
internal_pages     | 11
leaf_pages         | 2733
empty_pages        | 0
deleted_pages      | 0
avg_leaf_density   | 90.06
leaf_fragmentation | 0

test=# delete from tb3 where id%5=0;
DELETE 200000
test=# select * from pgstatindex('tb3_pkey');
-[ RECORD 1 ]------+---------
version            | 2
tree_level         | 2
index_size         | 22487040
root_block_no      | 412
internal_pages     | 11
leaf_pages         | 2733
empty_pages        | 0
deleted_pages      | 0
avg_leaf_density   | 90.06
leaf_fragmentation | 0

test=# select * from pgstattuple('tb3');
-[ RECORD 1 ]------+---------
table_len          | 68272128
tuple_count        | 800000
tuple_len          | 48800000
tuple_percent      | 71.48
dead_tuple_count   | 200000
dead_tuple_len     | 12200000
dead_tuple_percent | 17.87
free_space         | 38776
free_percent       | 0.06

test=#  vacuum tb3;
VACUUM
test=# select * from pgstattuple('tb3');
-[ RECORD 1 ]------+---------
table_len          | 68272128
tuple_count        | 800000
tuple_len          | 48800000
tuple_percent      | 71.48
dead_tuple_count   | 0
dead_tuple_len     | 0
dead_tuple_percent | 0
free_space         | 12838776
free_percent       | 18.81

test=# select * from pgstatindex('tb3_pkey');
-[ RECORD 1 ]------+---------
version            | 2
tree_level         | 2
index_size         | 22487040
root_block_no      | 412
internal_pages     | 11
leaf_pages         | 2733
empty_pages        | 0
deleted_pages      | 0
avg_leaf_density   | 72.11
leaf_fragmentation | 0

test=# select * from pgstattuple('tb3');
-[ RECORD 1 ]------+---------
table_len          | 68272128
tuple_count        | 800000
tuple_len          | 48800000
tuple_percent      | 71.48
dead_tuple_count   | 0
dead_tuple_len     | 0
dead_tuple_percent | 0
free_space         | 12838776
free_percent       | 18.81

test=# vacuum full tb3;
VACUUM
test=# select * from pgstattuple('tb3');
-[ RECORD 1 ]------+---------
table_len          | 54616064
tuple_count        | 800000
tuple_len          | 48800000
tuple_percent      | 89.35
dead_tuple_count   | 0
dead_tuple_len     | 0
dead_tuple_percent | 0
free_space         | 29388
free_percent       | 0.05


``` 



