---
title: "pg_pathman 分区表"
date: 2019-01-24T10:56:06+08:00
draft: false
---

#### 介绍

分区表的诉求在现实的生成中的意义不必多说，pg以前的实现方式多采用触发器，rules实现。数据量上来时性能明显不尽如意。   
虽然pg10 ，11 版本在分区表的特性上不断发力。但是性能啥还是不够给力。   
pg_pathman 分区表功能在目前的pg版本10.6 中优势还是非常明显的。   

在期待pg自身分区表特性的同时，当前的pg10中还是使用pg_pathman来实现分区功能吧。

##### pathman与pg11 对比

优点:
支持HASH和RANGE分区，后续会支持LIST分区 支持自动和手动的分区维护  
为分区表生成更有效的执行计划 通过引入两个自定义的执行计划节点RuntimeAppend & RuntimeMergeAppend，  
实现运行时动态添加分区到执行计划中 为新插入数据自动创建分区(只对RANGE分区) 提供用户callbacks接口处理创建分区事件。   
 提供在线分区实施(在线重定义)，父表数据迁移到子表，拆分， 合并分区
不足:   
不支持list分区;不支持二级分区;权限，索引，trigger等无法继承; 修改主键默认的seq需要重建分区。    

PG11内置分区   
优点:   
支持hash，range，list分区 支持多字段组合分区，支持表达式分区 支持创建主键，外键，索引，分区表自动继承。 支持update分区键 支持分区表DETACH，ATTACH，支持二级分区 分区自动创建   
Default partition Partition improvements   
不足:   
在主表添加权限，索引，trigger等无法继承 分区表不可以作为其他表的外键主表   


#### 分区表数量对插入数据的影响


https://www.jianshu.com/p/1cba77d18694

#### 参考

https://github.com/postgrespro/pg_pathman

https://github.com/digoal/blog/blob/362b84417ca8b7aaf1add31fe7689c347642bb9a/201610/20161024_01.md
 
