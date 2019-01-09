---
title: "Postgresql 指标查看"
date: 2018-11-06T10:53:52+08:00
draft: false
---

- 当前连接数

```
SELECT count(*) FROM pg_stat_activity WHERE NOT pid=pg_backend_pid();
 count 
-------
     3
(1 row)

```

- 数据库占用空间

```
select pg_size_pretty(pg_database_size('postgres'));
 pg_size_pretty 
----------------
 14 MB
(1 row)

or

\l+

```

- 数据库表或单条索引占用空间

```
select pg_size_pretty(pg_relation_size('t_name'));
 pg_size_pretty 
----------------
 24 kB
(1 行记录)

or

\d+
```

- 表中所有索引占有的空间

```
select pg_size_pretty(pg_indexes_size('t_name'));
 pg_size_pretty 
----------------
 280 kB
(1 行记录)
```

- 查看一条数据在数据库占用的空间

```
select pg_column_size('Let us go !!!');
 pg_column_size 
----------------
             14
(1 行记录)

```

- 其他

```
pg_stat_database
pg_stat_all_tables
pg_stat_sys_tables
pg_stat_user_tables
pg_stat_all_indexes
pg_stat_sys_indexes
pg_stat_user_indexes
```
