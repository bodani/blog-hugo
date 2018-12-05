---
title: "vacuum 垃圾回收器"
date: 2018-12-05T16:48:00+08:00
draft: false
---

#### 介绍

数据库总是不断地在执行删除，更新等操作。良好的空间管理非常重要，能够对性能带来大幅提高。在postgresql中用于维护数据库磁盘空间的工具是VACUUM，其重要的作用是删除那些已经标示为删除的数据并释放空间。
postgresql中执行deletei,update操作后，表中的记录只是被标示为删除状态，并没有释放空间，在以后的update或insert操作中该部分的空间是不能够被重用的。经过vacuum清理后，空间才能得到释放。

#### 文法

```
VACUUM [ ( { FULL | FREEZE | VERBOSE | ANALYZE | DISABLE_PAGE_SKIPPING } [, ...] ) ] [ table_name [ (column_name [, ...] ) ] ]
VACUUM [ FULL ] [ FREEZE ] [ VERBOSE ] [ table_name ]
VACUUM [ FULL ] [ FREEZE ] [ VERBOSE ] ANALYZE [ table_name [ (column_name [, ...] ) ] ]
```
#### 参数

##### FULL
Selects “full” vacuum, which can reclaim more space, but takes much longer and exclusively locks the table. This method also requires extra disk space, since it writes a new copy of the table and doesn't release the old copy until the operation is complete. Usually this should only be used when a significant amount of space needs to be reclaimed from within the table.

大招，需要更多的磁盘空间，空间将会被重新整理。　auto vacumm 只删除空间，并没有整理使空间更紧凑。

##### VERBOSE
Prints a detailed vacuum activity report for each table.
打印回收时每个table 执行细节

##### ANALYZE
Updates statistics used by the planner to determine the most efficient way to execute a query.

统计库

##### 例子

可以精确指定需要执行的table, 如果不指定被回收的对象，默认为当前database 下所有 table   
```
VACUUM (VERBOSE, ANALYZE) table_name;
```

将执行过程细节打印到文件中
```
psql -U user -d database  -c "vacuum FULL VERBOSE ANALYZE table"  > vacuum-01.log 2>&1
```
