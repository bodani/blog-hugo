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

实时备份恢复

https://github.com/ossc-db/pg_rman

https://github.com/wal-e/wal-e

https://github.com/wal-g/wal-g


由于原始库中存在extension 需要超级管理员权限进行恢复，恢复后将所有者变更为普通用户。
pg中没有方法可以将整个database 中table 的 owner 进行修改，使用如下方法进行批量修改


批量修改表和视图的所有者
```
DO $$DECLARE r record;
BEGIN
FOR r IN SELECT tablename/viewname FROM pg_tables/pg_views WHERE schemaname = 'public'
LOOP
    EXECUTE 'alter table '|| r.tablename/r.viewname ||' owner to new_owner;';
END LOOP;
END$$;
```

