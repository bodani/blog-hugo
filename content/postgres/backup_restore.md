---
title: "数据库备份和恢复"
date: 2018-10-30T10:18:57+08:00
draft: false
---
Postgres 数据库备份恢复命令

```
备份：pg_dump -U postgres -v -F c -Z 4 -f ***.backup dbname  9压缩率最狠
恢复：pg_restore -U postgres -v -j 8 -d dbname ***.backup   8是采用8个线程

备份表：pg_dump -U postgres -t tablename dbname > 33.sql
恢复表：psql -U postgres -d dbname < 33.sql
```
