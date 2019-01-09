---
title: "定时任务"
date: 2019-01-09T10:11:10+08:00
draft: false
---

#### Linux 系统中的定时任务

- 重复执行
- 一次执行

#### 重复执行

详见 /etc/crontab 配置

#### 一次执行

我们使用at 命令来管理Linux中单次执行任务

安装与启动
```
yum install at -y
systemctl start atd  
```

常用命令及参数讲解

- at和batch读取标准输入或一个指定文件，它们将会在稍后被执行。   
- at在指定的时间执行命令。   
- atq列出用户待处理作业（jobs），如果是超级用户，所有用户的（待处理）作业都将被列出。输出格式：作业号、日期、小时、队列和用户名。  
- atrm删除作业，由作业号标识。   

```
-t time run the job at time, given in the format [[CC]YY]MMDDhhmm[.ss]
-c cats the jobs listed on the command line to standard output.
```

#### 例子说明

新建一个定期任务 ctrl + d 退出
```
# at -t 201901091123.00
at> touch /home/11.23.at     
at> <EOT>
job 9 at Wed Jan  9 11:23:00 2019
# at -t 201901091124.00
at> touch /home/11.24.at 
at> <EOT>
job 10 at Wed Jan  9 11:24:00 2019
```

查看任务
```
# atq
9	Wed Jan  9 11:23:00 2019 a root
10	Wed Jan  9 11:24:00 2019 a root
```

根据任务id 查看任务具体内容

```
# at -c 9
```

删除任务

```
# atrm 9 
```

##### 扩展内容

```
# ll -a /var/spool/at/
总用量 12
drwx------  3 root root   75 1月   9 10:27 .
drwxr-xr-x. 9 root root   97 1月   9 09:49 ..
-rwx------  1 root root 3081 1月   9 10:27 a0000901896c6b
-rwx------  1 root root 3082 1月   9 10:28 a0000a01896c6c
-rw-------  1 root root    6 1月   9 10:27 .SEQ
drwx------  2 root root    6 1月   9 10:23 spool
```

.SEQ 序列

a0000901896c6b 任务内容，at -c id 输出的内容
