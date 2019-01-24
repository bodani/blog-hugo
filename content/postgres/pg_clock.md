---
title: "锁机制"
date: 2019-01-24T11:26:16+08:00
draft: false
---

当要增删改查表中的数据时，首先是要获得表上的锁，然后再获得行上的锁

 

postgresql中有8种表锁

最普通的是共享锁 share 和排他锁 exclusive

 

因为多版本的原因，修改一条语句的同时，允许了读数据，为了处理这种情况，又增加了两种锁”access share”和”acess excusive”，锁中的关键字 access 是与多版本相关的

 

为了处理表锁和行锁之间的关系，有了 意向锁 的概念，这时又加了两种锁，即 意向共享锁 和 意向排他锁 ，由于意向锁之间不会产生冲突，而且意向排它锁相互之间也不会产生冲突，于是又需要更严格一些的锁，这样就产生了“share update exclusive” 和 ”share row exclusive”

 

表级锁模式

表级锁模式
解释
ACCESS SHARE
只与“ACCESS EXCLUSIVE” 锁模式冲突;
 
查询命令（Select command）将会在它查询的表上获取”Access Shared” 锁,一般地，任何一个对表上的只读查询操作都将获取这种类型的锁。
ROW SHARE
与”Exclusive’和”Access Exclusive”锁模式冲突;
 
”Select for update”和”Select for share”命令将获得这种类型锁，并且所有被引用但没有 FOR UPDATE 的表上会加上”Access shared locks”锁。
ROW EXCLUSIVE
与 “Share,Shared roexclusive,Exclusive,Access exclusive”模式冲突;
 
“Update,Delete,Insert”命令会在目标表上获得这种类型的锁，并且在其它被引用的表上加上”Access shared”锁,一般地，更改表数据的命令都将在这张表上获得”Row exclusive”锁。
SHARE UPDATE EXCLUSIVE
”Share update exclusive,Share,Share row ,exclusive,exclusive,Access exclusive”模式冲突，这种模式保护一张表不被并发的模式更改和VACUUM;
 
“Vacuum(without full), Analyze ”和 “Create index concurrently”命令会获得这种类型锁。
SHARE
与“Row exclusive,Shared update exclusive,Share row exclusive ,Exclusive,Access exclusive”锁模式冲突,这种模式保护一张表数据不被并发的更改;
 
“Create index”命令会获得这种锁模式。
SHARE ROW EXCLUSIVE
与“Row exclusive,Share update exclusive,Shared,Shared row exclusive,Exclusive,Access Exclusive”锁模式冲突;
 
任何Postgresql 命令不会自动获得这种锁。
EXCLUSIVE
与” ROW SHARE, ROW EXCLUSIVE, SHARE UPDATE EXCLUSIVE, SHARE, SHARE ROW EXCLUSIVE, EXCLUSIVE, ACCESS EXCLUSIVE”模式冲突,这种索模式仅能与Access Share 模式并发,换句话说，只有读操作可以和持有”EXCLUSIVE”锁的事务并行；
 
任何Postgresql 命令不会自动获得这种类型的锁；
ACCESS EXCLUSIVE
与所有模式锁冲突(ACCESS SHARE, ROW SHARE, ROW EXCLUSIVE, SHARE UPDATE EXCLUSIVE, SHARE, SHARE ROW EXCLUSIVE, EXCLUSIVE, and ACCESS EXCLUSIVE),这种模式保证了当前只有一个事务访问这张表;“ALTER TABLE, DROP TABLE, TRUNCATE, REINDEX, CLUSTER, VACUUM FULL” 命令会获得这种类型锁，在Lock table 命令中，如果没有申明其它模式，它也是缺省模式。
 

表锁的冲突关系

Requested Lock Mode
Current Lock Mode
ACCESS SHARE
ROW SHARE
ROW 
EXCLUSIVE
SHARE UPDATE
EXCLUSIVE
SHARE
SHARE ROW 
EXCLUSIVE
EXCLUSIVE
ACCESS 
EXCLUSIVE
ACCESS SHARE
 
 
 
 
 
 
X
X
ROW SHARE
 
 
 
 
 
 
X
X
ROW EXCLUSIVE
 
 
 
 
X
X
X
X
SHARE UPDATE EXCLUSIVE
 
 
 
X
X
X
X
X
SHARE
 
 
X
X
 
X
X
X
SHARE ROW EXCLUSIVE
 
 
X
X
X
X
X
X
EXCLUSIVE
 
X
X
X
X
X
X
X
ACCESS EXCLUSIVE
X
X
X
X
X
X
X
X
 

表锁类型对应的数据库操作

锁类型
对应的数据库操作
ACCESS SHARE
select
ROW SHARE
select for update, select for share
ROW EXCLUSIVE
update,delete,insert
SHARE UPDATE EXCLUSIVE
vacuum(without full),analyze,create index concurrently
SHARE
create index
SHARE ROW EXCLUSIVE
任何Postgresql命令不会自动获得这种锁
EXCLUSIVE
任何Postgresql命令不会自动获得这种类型的锁
ACCESS EXCLUSIVE
alter table,drop table,truncate,reindex,cluster,vacuum full
 

表级锁命令（显式在表上加锁的命令）

testdb=# \h lock

Command:     LOCK

Description: lock a table

 

Syntax:

LOCK [ TABLE ] [ ONLY ] name [ * ] [, ...] [ IN lockmode MODE ] [ NOWAIT ]

 

where lockmode is one of:

    ACCESS SHARE | ROW SHARE | ROW EXCLUSIVE | SHARE UPDATE EXCLUSIVE

    | SHARE | SHARE ROW EXCLUSIVE | EXCLUSIVE | ACCESS EXCLUSIVE

 

注：

name：要锁定的现有表的锁名称(可选模式限定)。 如果在表名之前指定了ONLY，则仅该表被锁定 如果未指定ONLY，则表及其所有后代表(如果有)被锁定。

lock_mode：锁模式指定此锁与之冲突的锁。 如果未指定锁定模式，则使用最严格的访问模式ACCESS EXCLUSIVE。

nowait

当事务要更新表中的数据时，应该申请“ROW EXCLUSIVER”

 

行级锁模式

只有两种，共享锁和排他锁，或者可以说是“读锁” 或 “写锁“

由于多版本的实现，实际读取行数据时，并不会在行上执行任何锁

 

行级锁命令（显式加行锁）

SELECT …… FOR  { UPDATE | SHARE } [OF table_name[,……]] [ NOWAIT]

备注：

1）指定 OF table_name，则只有被指定的表会被锁定

2）例外情况，主查询中引用了WITH查询时，WITH查询中的表不被锁定

3）如果需要锁定WITH查询中的表，需在WITH查询内指定FOR UPDATA或FOR SHARE
