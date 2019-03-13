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

- checkpoint_segments  WAL log的最大数量，系统默认值是3。超过该数量的WAL日志，会自动触发checkpoint。
- checkpoint_timeout  系统自动执行checkpoint之间的最大时间间隔。系统默认值是5分钟。
- checkpoint_completion_target 该参数表示checkpoint的完成时间占两次checkpoint时间间隔的比例，系统默认值是0.5,也就是说每个checkpoint需要在checkpoints间隔时间的50%内完成。
- checkpoint_warning 系统默认值是30秒，如果checkpoints的实际发生间隔小于该参数，将会在server log中写入写入一条相关信息。可以通过设置为0禁用。

##### 应用

预防wal写放大

http://www.postgres.cn/news/viewone/1/273

更多细节说明

https://yq.aliyun.com/articles/582847
