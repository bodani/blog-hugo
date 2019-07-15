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

- 数据库表(不包括索引)或单条索引占用空间

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

- 表和索引占用总空间

```
select pg_size_pretty(pg_total_relation_size('t_name'));
 pg_size_pretty
----------------
 380 kB
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

- 查出所有表（包含索引）并排序

```
SELECT table_schema || '.' || table_name AS table_full_name, pg_size_pretty(pg_total_relation_size('"' || table_schema || '"."' || table_name || '"')) AS size
FROM information_schema.tables
ORDER BY
pg_total_relation_size('"' || table_schema || '"."' || table_name || '"') DESC limit 10;
```

- 查出表大小按大小排序并分离data与index

```
SELECT
table_name,
pg_size_pretty(table_size) AS table_size,
pg_size_pretty(indexes_size) AS indexes_size,
pg_size_pretty(total_size) AS total_size
FROM (
SELECT
table_name,
pg_table_size(table_name) AS table_size,
pg_indexes_size(table_name) AS indexes_size,
pg_total_relation_size(table_name) AS total_size
FROM (
SELECT ('"' || table_schema || '"."' || table_name || '"') AS table_name
FROM information_schema.tables
) AS all_tables
ORDER BY total_size DESC
) AS pretty_sizes;
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
