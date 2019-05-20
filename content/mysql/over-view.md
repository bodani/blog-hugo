---
title: "Mysql 入门"
date: 2019-02-21T14:31:26+08:00
draft: false
---
#### 安装 & 启动

##### 安装
```
rpm -i https://dev.mysql.com/get/mysql80-community-release-el7-3.noarch.rpm
yum install mysql-community-server.x86_64
```

##### 启动
```
systemctl start mysqld
systemctl enable mysqld
```

##### 查看临时密码
```
sudo grep 'temporary password' /var/log/mysqld.log
2019-05-15T06:42:54.826106Z 5 [Note] [MY-010454] [Server] A temporary password is generated for root@localhost: DVjSsl-ZX5f7
```

##### 修改密码
```
mysql -uroot -p
ALTER USER 'root'@'localhost' IDENTIFIED BY 'MyNewPass4!';
```
#### 常用命令

##### 连接
```
mysql -h127.0.0.1 -P 3306  -uroot -p

```


#### 权限管理 

mysql登陆用户权限校验主要是通过用户名密码+访问来源主机方式

##### 创建用户
```
CREATE USER 'finley'@'localhost'
  IDENTIFIED BY 'password';
```
##### 赋权限
```
GRANT ALL
  ON *.*
  TO 'finley'@'localhost'
  WITH GRANT OPTION;
```
##### 收回权限
```
REVOKE ALL
  ON *.*
  FROM 'finley'@'localhost';
```
##### 查看
```
select host,user from mysql.user;

SHOW GRANTS FOR 'finley'@'localhost';
```
##### 删除
```
DROP USER 'finley'@'localhost';
```

##### 


#### 配置管理

```
查看所用配置参数
show (global) variables;

查看某个指定的配置参数
show variables like '%max_heap_table_size%';  
+---------------------+----------+   
| Variable_name       | Value    |
+---------------------+----------+
| max_heap_table_size | 16777216 |
+---------------------+----------+

设置参数
set max_heap_table_size = 167772160;                                                                                                                             
```
#### 数据类型 

#### 数据导入导出 

#### 高可用&主从架构

#### 监控 

#### explain执行计划  

#### 增量，全量备份

#### 周边工具 

- 客户端命令自动补齐工具 [mycli](https://github.com/dbcli/mycli)
 
