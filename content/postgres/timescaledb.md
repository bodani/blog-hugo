---
title: "TimescaleDB 时序数据库"
date: 2019-01-30T10:20:51+08:00
draft: false
---

时序数据库
https://github.com/timescale/timescaledb


数据库配置
https://github.com/timescale/timescaledb-tune


copy并行导入数据
https://github.com/timescale/timescaledb-parallel-copy


#### 常用方法

创建拓展
```
CREATE EXTENSION timescaledb;
```

创建一个普通的表
```
CREATE TABLE conditions (
  time        TIMESTAMPTZ       NOT NULL,
  location    TEXT              NOT NULL,
  temperature DOUBLE PRECISION  NULL,
  humidity    DOUBLE PRECISION  NULL
);
```

转换成时序数据库表
```
SELECT create_hypertable('conditions', 'time');
```
- conditions 表名
- time 时序字段

修改时序间隔 对新表生效
```
SELECT set_chunk_time_interval('conditions', INTERVAL '24 hours');
```

查看分区
```
SELECT show_chunks('conditions');

SELECT show_chunks('conditions', older_than => INTERVAL '3 months')

SELECT show_chunks('conditions', older_than => DATE '2017-01-01');

SELECT show_chunks(newer_than => INTERVAL '3 months');

SELECT show_chunks(older_than => INTERVAL '3 months', newer_than => INTERVAL '4 months');

```

自动删除
```
添加规则
SELECT add_drop_chunks_policy('conditions', INTERVAL '6 months');
删除规则
SELECT remove_drop_chunks_policy('conditions');
```

#### 实验


