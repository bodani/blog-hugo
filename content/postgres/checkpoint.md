---
title: "checkpoint 检查点"
date: 2019-03-13T15:57:25+08:00
draft: false
---
##### 作用

一般checkpoint会将某个时间点之前的脏数据全部刷新到磁盘，以实现数据的一致性与完整性。其主要目的是为了缩短崩溃恢复时间。

##### 触发

- 超级用户（其他用户不可）执行CHECKPOINT命令
- 数据库shutdown
- 数据库recovery完成
- XLOG日志量达到了触发checkpoint阈值
- 周期性地进行checkpoint
- 需要刷新所有脏页

##### 相关参数

- checkpoint_segments  WAL log的最大数量，系统默认值是3。超过该数量的WAL日志，会自动触发checkpoint。 新版(9.6)使用min_wal_size, max_wal_size  来动态控制wal日志
- checkpoint_timeout  系统自动执行checkpoint之间的最大时间间隔。系统默认值是5分钟。
- checkpoint_completion_target 该参数表示checkpoint的完成时间占两次checkpoint时间间隔的比例，系统默认值是0.5,也就是说每个checkpoint需要在checkpoints间隔时间的50%内完成。
- checkpoint_warning 系统默认值是30秒，如果checkpoints的实际发生间隔小于该参数，将会在server log中写入一条相关信息。可以通过设置为0禁用。

##### 应用

预防wal写放大


##### 如何判断是否需要优化WAL？

关于如何判断是否需要优化WAL，可以通过分析WAL，然后检查下面的条件，做一个粗略的判断：

- FPI比例高于70%
- HOT_UPDATE比例低于70%

FPI查看方法

```
/usr/pgsql-10/bin/pg_waldump -z -p /var/lib/pgsql/10/data/pg_wal/ -t 2  -s 15/56098120 -e 15/56098200

-z 统计信息

-p wal path

-t timeline

-s sart lsn

-e end lsn

获取wal lsn psql -c "checkpoint;select pg_current_wal_lsn" 
```

HOT_UPDATE 查看

```
select * from pg_stat_user_tables ;
```

以上仅仅是粗略的经验值，仅供参考。并且这个FPI比例可能不适用于低写负载的系统，低写负载的系统FPI比例一定非常高，但是，低写负载系统由于写操作很少，因此FPI比例即使高一点也没太大影响。


优化WAL及副作用


- 延长checkpoint时间间隔
导致crash恢复时间变长。crash恢复时需要回放的WAL日志量一般小于max_wal_size的一半，WAL回放速度(wal_compression=on时)一般是50MB/s~150MB/s之间。可以根据可容忍的最大crash恢复时间，估算出允许的max_wal_size的最大值。

- 调整fillfactor
过小的设置会浪费存储空间，这个不难理解。另外，对于频繁更新的表，即使把fillfactor设成100%，每个page里还是要一部分空间被dead tuple占据，不会比设置成一个合适的稍小的fillfactor更节省空间。

- 设置wal_compression=on
需要额外占用CPU资源进行压缩，但影响不大

http://www.postgres.cn/news/viewone/1/273

更多细节说明

https://yq.aliyun.com/articles/582847
