---
title: "表空间膨胀"
date: 2019-05-22T17:26:45+08:00
draft: false
---

#### 背景介绍

由于mvcc机制，数据被删除后只是被标记为删除，实际空间没有被释放，这是表空间膨胀的根本原因。

目前用于解决表空间膨胀方式有如下方式

1 删除dead tuple 

- vacuum  ,tuple被清理。数据库可以自动执行autovacuum
- vacuum full ,tuple被清理并且空间连续紧凑。弊端，在执行过程中会锁表。应用不可用
- 为了避免锁表的影响，提供的pg_squeeze拓展。使用逻辑复制。[pg_repack拓展](https://www.timbotetsu.com/blog/postgresql-bloatbusters/)，使用了触发器，影响业务的性能。

2 fillfactor

3 vacuum_defer_cleanup_age > 0, 是以事务为单位。配合pg_resetwal 可以做到flashback
```
代价1，主库膨胀，因为垃圾版本要延迟若干个事务后才能被回收。
代价2，重复扫描垃圾版本，重复耗费垃圾回收进程的CPU资源。（n_dead_tup会一直处于超过垃圾回收阈值的状态，从而autovacuum 不断唤醒worker进行回收动作）。
当主库的 autovacuum_naptime=很小的值，同时autovacuum_vacuum_scale_factor=很小的值时，尤为明显。
代价3，如果期间发生大量垃圾，垃圾版本可能会在事务到达并解禁后，爆炸性的被回收，产生大量的WAL日志，从而造成WAL的写IO尖刺。
```
4 reindex 从新建立索引，不要忽略表膨胀中索引的影响，通常来说索引所占的空间和维护成本要高于数据表，在pg version 12版本中预计reindex时不需要锁表。

###### 处理完毕后需要重新生成统计信息

```
 ANALYZE;
```

在执行以上操作时建议设置

```
set maintenance_work_mem = '10GB';
```

#### 监控表空间膨胀

pgstattuple提供了pgstatetuple()和pgstatindex()两个统计表和索引的方法，较PostgreSQL系统表pg_class的表统计信息，pgstatetuple()还统计了表中的dead tuples。

https://www.postgresql.org/docs/current/pgstattuple.html

创建拓展
```
create extension pgstattuple ;
```

```
表
test=> SELECT * FROM pgstattuple('tablename');
-[ RECORD 1 ]------+-------
table_len          | 458752
tuple_count        | 1470
tuple_len          | 438896
tuple_percent      | 95.67
dead_tuple_count   | 11
dead_tuple_len     | 3157
dead_tuple_percent | 0.69
free_space         | 8932
free_percent       | 1.95

字段说明

table_len	bigint	Physical relation length in bytes 等价于 pg_relation_size()
tuple_count	bigint	Number of live tuples 
tuple_len	bigint	Total length of live tuples in bytes
tuple_percent	float8	Percentage of live tuples
dead_tuple_count	bigint	Number of dead tuples
dead_tuple_len	bigint	Total length of dead tuples in bytes
dead_tuple_percent	float8	Percentage of dead tuples
free_space	bigint	Total free space in bytes
free_percent	float8	Percentage of free space
三部分 live(存活tuple) dead(被标记删除tuple) free(剩余空间 应该是dead tuple ,vacuum 后被释放的, filltor 设置)

所用表
select tablename, (x).* from pg_tables ,LATERAL (select * from pgstattuple(tablename)) as X where schemaname = 'public' order by tuple_percent asc;

索引

test=> SELECT * FROM pgstatindex('index_name');
-[ RECORD 1 ]------+------
version            | 2
tree_level         | 0
index_size         | 16384
root_block_no      | 1
internal_pages     | 0
leaf_pages         | 1
empty_pages        | 0
deleted_pages      | 0
avg_leaf_density   | 54.27 (fillfator 默认90 ，该值接近90比较正常)
leaf_fragmentation | 0

字段说明
version	integer	B-tree version number
tree_level	integer	Tree level of the root page
index_size	bigint	Total index size in bytes
root_block_no	bigint	Location of root page (zero if none)
internal_pages	bigint	Number of “internal” (upper-level) pages
leaf_pages	bigint	Number of leaf pages
empty_pages	bigint	Number of empty pages
deleted_pages	bigint	Number of deleted pages
avg_leaf_density	float8	Average density of leaf pages
leaf_fragmentation	float8	Leaf page fragmentation

```
#### 简单使用

```
--- 生成测试数据

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

--- 创建拓展

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



