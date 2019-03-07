---
title: "数据库参数"
date: 2018-11-27T09:57:27+08:00
draft: false
---

#### postgres 数据库参数该如何设置

####  性能参数[pgtune](https://github.com/le0pard/pgtune) [pgconfig](https://www.pgconfig.org/)  

####  [日志参数](postgres/log/)  

##### 管理
```
listen_addresses = "*"             # 连接访问控制，哪些ip可以访问， * 全部。 结合pg_hba.conf , iptables设置。
superuser_reserved_connections = 3 # 预留给超级管理员的连接数。
port = 5432                        # 默认访问端口
```

#### 成本因子 

```
# - Planner Cost Constants -
#seq_page_cost = 1.0                    # measured on an arbitrary scale 顺序扫描
random_page_cost = 1.1                  # same scale as above            随机扫描。HDD 4 ;SSD 1.1; 由于SSD没有磁盘寻道时间，顺序扫描和随机扫描的差距不是那么大。比例设置的相近即可。 
#cpu_tuple_cost = 0.01                  # same scale as above
#cpu_index_tuple_cost = 0.005           # same scale as above
#cpu_operator_cost = 0.0025             # same scale as above
#parallel_tuple_cost = 0.1              # same scale as above
#parallel_setup_cost = 1000.0   # same scale as above
#min_parallel_table_scan_size = 8MB
#min_parallel_index_scan_size = 512kB
effective_cache_size = 666666          # 系统总内存减去数据库shared_buffer减去其他应用占有的内存。 理解为数据可加载到内存的大小。
```

#### TCP 连接

Linux 中tcp默认连接超时时间2小时,如果2个小时没有数据包则认为该连接为空闲状态，系统自动关闭。

```
# - TCP Keepalives -
# see "man 7 tcp" for details

#tcp_keepalives_idle = 60                # TCP_KEEPIDLE, in seconds;
                                        # 0 selects the system default
#tcp_keepalives_interval = 10            # TCP_KEEPINTVL, in seconds;  发个心跳数据包，告诉系统我没有空闲
                                        # 0 selects the system default
#tcp_keepalives_count = 6               # TCP_KEEPCNT;
                                        # 0 selects the system default
```

#### 修改

```
postgresql.conf 服务器启动时默认读取的配置

postgresql.auto.conf 优先级高于postgresql.conf 9.4后引入,对标oracle sfile pfile 。　文件不能修改,需要通过ALTER SYSTE　修改，ALTER SYSTE　RESET | DEFAULT 删除
```

##### 策略　

postgresql.conf 参数为默认值,不做修改，优化参数通过　postgresql.auto.conf 修改，一目了然。(个人习惯)

#### 查看

SELECT name,setting,vartype,boot_val,min_val,max_val,reset_val FROM pg_settings;

show all;

---

#### work_mem

这些内存大小被用来完成内部排序与哈希表操作。
如果未分配足够内存，会导致物理I/O。
work_mem这个值是针对每个session的，所以不能设置的过大。

##### 实验
```
创建测试表
postgres=# create table myt (id serial);  
CREATE TABLE

插入测试数据  
postgres=# insert into myt select generate_series(1,1000000);  
INSERT 0 1000000  

设置当前session work_mem

postgres=#set work_me '64kb';
SET  
postgres=# show work_mem;  
 work_mem  
----------  
 64kB  
(1 row)

查看临时文件占用情况
select temp_files, temp_bytes from pg_stat_database
 temp_files | temp_bytes  
------------+------------  
          0 |          0  
(1 row)  

执行测试
select * from (select * from myt order by id) t limit 1000; 

再次查看临时文件占用情况
select temp_files, temp_bytes from pg_stat_database
 temp_files | temp_bytes
------------+------------
          1 |   14016512 
(1 row)

设置当前session work_mem

postgres=#set work_mem = '16MB';  
SET

执行测试
select * from (select * from myt order by id) t limit 1000;

再次查看临时文件占用情况
select temp_files, temp_bytes from pg_stat_database
 temp_files | temp_bytes
------------+------------
          1 |   14016512
```
没有新增临时文件 , 说明work_mem充足

---

#### maintainance_work_mem

主要用于analyzing，vacuum，create index, reindex等。

如需进行如上操作时请适当调整maintainance_work_mem 值，提高效率

##### 实验
方法与上面类似，在统计表中观察临时文件使用情况。




