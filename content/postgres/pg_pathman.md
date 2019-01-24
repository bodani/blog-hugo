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

####

https://github.com/postgrespro/pg_pathman

https://github.com/digoal/blog/blob/362b84417ca8b7aaf1add31fe7689c347642bb9a/201610/20161024_01.md
 
