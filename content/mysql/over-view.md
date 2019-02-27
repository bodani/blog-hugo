---
title: "Mysql 入门"
date: 2019-02-21T14:31:26+08:00
draft: false
---

#### 安装 & 启动

#### 常用命令

#### 权限管理 

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
 
