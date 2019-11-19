---
title: "postgres 12"
date: 2019-11-19T08:43:36+08:00
draft: false
---

##### 安装&启动

```
#下载源
yum install https://download.postgresql.org/pub/repos/yum/reporpms/EL-7-x86_64/pgdg-redhat-repo-latest.noarch.rpm
#安装服务
yum install postgresql12 postgresql12-server
#初始化
/usr/pgsql-12/bin/postgresql-12-setup initdb
#启动服务
systemctl enable postgresql-12
systemctl start postgresql-12
```

##### 流复制

```
#从机 建立从库
pg_basebackup -h 10.1.30.13 -U postgres -F p -P -R -D /var/lib/pgsql/12/data/ --checkpoint=fast -l postgresback
#从库升级为主库
sudo su postgres -c  "/usr/pgsql-12/bin/pg_ctl promote -D /var/lib/pgsql/12/data/"
```

- recovery.conf 配置文件不再支持，此文件中的参数合并到 postgresql.conf(postgresql.auto.conf) Recovery Target, 若 recovery.conf 存在，数据库无法启动
- 新增 recovery.signal 标识文件，表示数据库处于 recovery 模式
- 新增加 standby.signal 标识文件，表示数据库处于 standby 模式
- trigger_file 参数更名为 promote_trigger_file
- standby_mode 参数不再支持

在postgres 12 版本中新增一个激活从库为主库的方式。pg_promote 函数，相比原有的两种方式，这种方法的优点在于不需要登陆到实体机上，可远程通过sql进行操作。
pg_promote() 函数有两个参数:

- wait: 表示是否等待备库的 promotion 完成或者 wait_seconds 秒之后返回成功，默认值为 true。
- wait_seconds: 等待时间，单位秒，默认 60

流复制主备切换主要步骤如下:

步骤1 关闭主库
步骤2 激活备库: 三种方式任选一种: 1) pg_ctl promote 命令方式; 2) 创建触发器文件方式; 3) pg_promote()函数方式。
步骤3 老主库角色转换成备库: 在老主库主机 pghost1 的 $PGDATA 目录下创建 standby.signal 标识文件,postgresql.auto.conf 类似于以前版本的recovery.conf。
步骤4 启动老主库并验证

[具体操作](https://postgres.fun/20190719084100.html)

##### 其他

https://yq.aliyun.com/articles/720247?spm=a2c4e.11153940.0.0.48cf2f79tPuOrL

https://github.com/digoal/blog/blob/0ef02248fe7419c55a98a425feefd2421ad25537/201906/20190624_02.md
 
