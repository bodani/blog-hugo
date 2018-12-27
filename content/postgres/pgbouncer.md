---
title: "pgbouncer"
date: 2018-12-27T09:00:49+08:00
draft: false
---

#### 背景介绍

- Pgbouncer是一个针对PostgreSQL数据库的轻量级连接池  
- pgbouncer 的目标是降低因为新建到 PostgreSQL 的连接而导致的性能损失   

##### 优势  
内存消耗低(默认为2k/连接)，因为Bouncer不需要每次都接受完整的数据包。   
Postgres的连接是进程模型，pogbouncer 使用libevent进行socket 通信。  

总结： 数据访问过程中建立连接很耗资源，pgboucer就是为了减少数据访问中的建立连接次数，重复利用已建立的连接进而缓解数据库压力。

#### 三种连接池模型

- session 会话级 ； 比较友好
- transaction 事务级； 比较激进
- statement 一个sql ； 客户端强制autocommit 模式

#### 安装

```
yum list pgbouncer.x86_64
pgbouncer.x86_64                                                                                                                     1.9.0-1.rhel7

yum install pgbouncer.x86_64 -y
```

#### 简单配置
```
cat /etc/pgbouncer/pgbouncer.ini | grep -v '^;' | grep -v '^$' 
[databases]
postgres= host=127.0.0.1 port=5432 user=postgres dbname=postgres connect_query='select 1' pool_size=40
zabbix= host=10.1.88.74 port=5432 dbname=zabbix  connect_query='select 1' pool_size=40
[pgbouncer]
logfile = /var/log/pgbouncer/pgbouncer.log
pidfile = /var/run/pgbouncer/pgbouncer.pid
listen_addr = 0.0.0.0
listen_port = 6432
auth_type = md5
auth_file = /etc/pgbouncer/userlist.txt
admin_users = postgres
stats_users = stats, postgres
pool_mode = session
server_reset_query = DISCARD ALL
max_client_conn = 100
default_pool_size = 20
```

说明:

##### [databases] 
主要思想承上启下的作用相当于代理，呈上对访问者，启下对后端数据库。   
第一项的名称是pgbouncer对外提供的数据库名 postgres ,等号后面是连接后端数据库名信息
关于user 配置后面细说  
##### [pgbouncer] 
pgbouncer自身的配置
max_client_conn= 


