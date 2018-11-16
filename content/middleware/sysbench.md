---
title: "Sysbench 测试"
date: 2018-11-16T19:25:45+08:00
draft: false
---

##### 下载安装 1.0.9

```
$ yum install sysbench -y

$ sysbench --version
  sysbench 1.0.9

```

##### sysbench对数据库进行压力测试的过程

- prepare  这个阶段是用来做准备的、建立好测试用的表、并向表中填充数据。
- run      这个阶段是才是去跑压力测试的SQL
- cleanup 这个阶段是去清除数据的、也就是prepare阶段初始化好的表要都drop掉。

```
sysbench --mysql-host=localhost --mysql-port=3306 --mysql-user=sbtest --mysql-password=123456 --mysql-db=tempdb oltp_insert prepare

sysbench --mysql-host=localhost --mysql-port=3306 --mysql-user=sbtest --mysql-password=123456 --mysql-db=tempdb oltp_insert run

sysbench --mysql-host=localhost --mysql-port=3306 --mysql-user=sbtest     --mysql-password=123456 --mysql-db=tempdb oltp_insert cleanup

```
