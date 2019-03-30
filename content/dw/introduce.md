---
title: "数据仓库介绍"
date: 2018-09-21T09:54:50+08:00
draft: false
---

##### 数据库与数据仓库的区别

- 数据库是面向事务的设计，数据仓库是面向主题设计的。
- 数据库一般服务于业务系统的，数据仓库一般是服务于分析系统的。
- 数据库一般存储在线交易数据，数据仓库存储的一般是历史数据。
- 数据库设计是尽量避免冗余，数据仓库在设计是有意引入冗余。
- 数据库是为捕获数据而设计，数据仓库是为分析数据而设计。
- 数据库一般会对数据进行增删改查，数据仓库一般只对进行增和查，基本不会修改数据。

1. 全量表：每天的所有的最新状态的数据，
2. 增量表：每天的新增数据，增量数据是上次导出之后的新数据。
3. 拉链表：维护历史状态，以及最新状态数据的一种表，拉链表根据拉链粒度的不同，实际上相当于快照，只不过做了优化，去除了一部分不变的记录而已,通过拉链表可以很方便的还原出拉链时点的客户记录。
4. 流水表： 对于表的每一个修改都会记录，可以用于反映实际记录的变更。 
