---
title: "citus 数据库分库"
date: 2019-01-29T13:19:26+08:00
draft: false
---

##### 数据库分库调研

- Greenplum  更适用于AP场景
- PGXL PGXC  社区不活跃，沟通问题反馈时间长。没找到用户群体. 在此基础上发展的有亚信antdb，腾讯tbase。没有那个研发实力，算了吧。
- citus      插件方式，无侵入。很多牛X的特性企业版才支持。主要强调多租户。 
- mycat      mysql支派，阿里开源（抛弃）项目。主要是对sql语句的拦截，需要对业务理解透彻又要懂mycat，入侵太强。
- bdr。      2ndquadrant 
- 其他       由数据库触发器实现的直接略过

citus 开源社区版，如何分库及扩容，ha

主要思路是通过修改系统的分区表，手动进行分库。   
ha 数据库自身的主从流复制。 

##### 实验目标  
- 加入数据库几点进行扩容
- 删除数据库节点进行缩容
- 模拟任意节点宕机观察ha特性
- 压力测试判断增加主机节点与数据库整体处理能力之间的线形关系
