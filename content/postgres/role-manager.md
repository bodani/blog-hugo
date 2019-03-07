---
title: "权限管理"
date: 2018-12-20T09:54:28+08:00
draft: false
---

##### 创建用户
```
# user 与 role 区别 ， user 具有login权限
postgres=# create user tester with password '123456';
CREATE ROLE
```

##### 创建数据库,并关联所有者
```
postgres=# create database test owner tester ;
CREATE DATABASE
```

##### 变更数据库用户所有者 
```
postgres=# alter database test owner to tester;
ALTER DATABASE
```

##### 修改用户&数据库
```
#用户连接数
postgres=# alter user tester connection limit 100;
ALTER ROLE
#数据库连接数
postgres=# alter database test connection limit 100;
ALTER DATABASE

#用户其他属性修改
postgres=# alter user tester 
BYPASSRLS           CREATEDB            ENCRYPTED PASSWORD  LOGIN               NOCREATEDB          NOINHERIT           NOREPLICATION       PASSWORD            REPLICATION         SET                 VALID UNTIL         
CONNECTION LIMIT    CREATEROLE          INHERIT             NOBYPASSRLS         NOCREATEROLE        NOLOGIN             NOSUPERUSER         RENAME TO           RESET               SUPERUSER           WITH 

#数据库其他属性修改
postgres=# alter database test 
ALLOW_CONNECTIONS  CONNECTION LIMIT   IS_TEMPLATE        OWNER TO           RENAME TO          RESET              SET 
```

##### sql批量修改table/view的owner

```
DO $$DECLARE r record;
BEGIN
FOR r IN SELECT tablename/viewname FROM pg_tables/pg_views WHERE schemaname = 'public'
LOOP
    EXECUTE 'alter table '|| r.tablename/r.viewname ||' owner to new_owner;';
END LOOP;
END$$;
```


##### 查看用户&数据库
```
#查看数据库
postgres=# \l+ test 
                                         数据库列表
 名称 | 拥有者 | 字元编码 |  校对规则   |    Ctype    | 存取权限 | 大小  |   表空间   | 描述 
------+--------+----------+-------------+-------------+----------+-------+------------+------
 test | tester | UTF8     | en_US.UTF-8 | en_US.UTF-8 |          | 14 MB | pg_default | 
(1 行记录)

#查看表

postgres=# \d+

#查看用户
postgres=# \dg
                                 角色列表
     角色名称     |                    属性                    | 成员属于 
------------------+--------------------------------------------+----------
 postgres         | 超级用户, 建立角色, 建立 DB, 复制, 绕过RLS | {}
 tester           | 10个连接                                   | {}
```


