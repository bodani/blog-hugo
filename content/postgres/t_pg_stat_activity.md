---
title: "数据库视图之 pg_stat_activity"
date: 2019-08-23T13:47:12+08:00
draft: false
---

##### 介绍
当需要了解数据库当前运行状态或需要排查问题时，首先需要查看的就是pg_stat_activity。该视图中包含了你想知道的数据库连接信息，正在执行的有哪些sql，并处于何状态。

One row per server process, showing information related to the current activity of that process, such as state and current query.

每一行都表示一个系统进程，显示与当前会话的活动进程的一些信息，比如当前回话的状态和查询等。

##### 字段解读



|Column	 | Type | Description |
|:----|:----|:----|
|datid|	oid|	OID of the database this backend is connected to| 
|datname|	name|	Name of the database this backend is connected to|
|pid|	integer|	Process ID of this backend|
|usesysid|	oid|	OID of the user logged into this backend|
|usename|	name|	Name of the user logged into this backend|
|application_name|	text|	Name of the application that is connected to this backend|
|client_addr|	inet|	IP address of the client connected to this backend. If this field is null, itindicates(表明) either that the client is connected via a Unixsocket(插座) on the server machine or that this is aninternal(内部的) process such as autovacuum.|
|client_hostname|	text|	Host name of the connected client, as reported by a reverse(相反) DNS lookup(查找) of client_addr. This field will only be non-null for IP connections, and only whenlog_hostname is enabled.|
|client_port|	integer|	TCP port number that the client is using for communication with this backend, or-1 if a Unixsocket(插座) is used|
|backend_start|	timestamp(时间戳) with time zone|	Time when this process was started, i.e., when the client connected to the server|
|xact_start|	timestamp with time zone|	Time when this process' current transaction(交易) was started, or null if no transaction is active. If the current query is the first of its transaction, this column is equal to the query_start column.|
|query_start|	timestamp with time zone|	Time when the currently active query was started, or if state is not active, when the last query was started|
|state_change|	timestamp(时间戳) with time zone|	Time when the state was last changed|
|waiting|	boolean|	True if this backend is currently waiting on a lock|
|state|	text| | 
|query|	text|	Text of this backend's most recent query. Ifstate isactive this field shows the currently executing query. In all other states, it shows the last query that was executed.|

###### state 字段详解

- active: The backend isexecuting(实行) a query. 正在执行中
- idle: The backend is waiting for a new client command. 连接已经建立等待客户端命令
- idle in transaction: The backend is in atransaction(交易), but is not currentlyexecuting(实行) a query. 事务已经begin 尚未commit
- idle in transaction (aborted): This state is similar toidle in transaction, except one of thestatements(声明) in the transaction caused an error. 事务中断
- fastpath function call: The backend is executing a fast-path function. 
- disabled: This state is reported iftrack_activities is disabled in this backend.

###### 当前正在执行的查询所处的状态

```
select datname, count(*) AS open,count(*) FILTER (WHERE state = 'active') AS active,
                count(*) FILTER(WHERE state = 'idle') AS idle ,
                count(*) FILTER(WHERE state = 'idle in transaction') AS idle_in_trans
                FROM pg_stat_activity GROUP BY ROLLUP(1);
```

######  一直有连接长时间处于idle in transaction的问题

配置postgresql.conf
```
idle_in_transaction_session_timeout=30000
```

###### query 内容显示不全

配置postgresql.conf
```
track_activity_query_size=32768
```

###### 杀死已挂掉的连接

```
select pg_terminate_backend(pid)
```

###### 取消正在执行的sql pid (不会释放连接，只会取消sql查询语句)
```
SELECT pg_cancel_backend(pid);
```
