---
title: "数据库日志"
date: 2018-12-04T15:45:33+08:00
draft: false
---

#### 介绍

PostgreSQL有3种日志，分别是pg_log（数据库运行日志）、pg_xlog（WAL 日志，即重做日志）、pg_clog（事务提交日志，记录的是事务的元数据）
postgres 10 版本将文件目录结构改为 log，pg_wal，pg_xact
log默认是关闭的，需要设置其参数。wal和xact都是强制打开的，无法关闭。
本文主要介绍　log 能

#### 配置
启用pg_log并配置日志参数
```
ALTER SYSTEM SET
 log_destination = 'csvlog';
ALTER SYSTEM SET
 logging_collector = on;
ALTER SYSTEM SET
 log_filename = 'postgresql-%Y-%m-%d_%H%M%S.log';
ALTER SYSTEM SET
 log_rotation_age = '1d';
ALTER SYSTEM SET
 log_rotation_size = '100MB';
ALTER SYSTEM SET
 log_min_messages = 'info';
```

##### 记录执行慢的SQL
```
ALTER SYSTEM SET
 log_min_duration_statement = 60;
ALTER SYSTEM SET
 log_checkpoints = on;
ALTER SYSTEM SET
 log_connections = on;
ALTER SYSTEM SET
 log_disconnections = on;
ALTER SYSTEM SET
 log_duration = on;
ALTER SYSTEM SET
 log_line_prefix = '%m';
```

##### 监控数据库中长时间的锁
```
ALTER SYSTEM SET
 log_lock_waits = on;
```
###### 记录DDL操作
```
ALTER SYSTEM SET
 log_statement = 'ddl';
```
