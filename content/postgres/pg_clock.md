---
title: "锁机制"
date: 2019-01-24T11:26:16+08:00
draft: false
---
https://blog.csdn.net/pg_hgdb/article/details/79403651

```
查看被堵塞的任务
select * from pg_locks where not granted;
 locktype | database | relation | page | tuple | virtualxid | transactionid | classid | objid | objsubid | virtualtransaction | pid | mode | granted | fastpath 
----------+----------+----------+------+-------+------------+---------------+---------+-------+----------+--------------------+-----+------+---------+----------
(0 行记录)

查看等待锁信息，是被谁堵塞了
select pg_blocking_pids(pid);
 pg_blocking_pids 
------------------
 {}

终止进程

select pg_cancel_backend(pid);  # select 

select pg_terminate_backend(pid); # update insert delete 
```



