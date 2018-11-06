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

```

命令 \l+

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
