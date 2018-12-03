---
title: "数据库参数"
date: 2018-11-27T09:57:27+08:00
draft: false
---

#### postgres 数据库参数该如何设置

https://github.com/le0pard/pgtune

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
