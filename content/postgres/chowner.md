---
title: "sql批量修改table/view的owner"
date: 2018-12-20T09:54:27+08:00
draft: false
---

```
DO $$DECLARE r record;
BEGIN
FOR r IN SELECT tablename/viewname FROM pg_tables/pg_views WHERE schemaname = 'public'
LOOP
    EXECUTE 'alter table '|| r.tablename/r.viewname ||' owner to new_owner;';
END LOOP;
END$$;
```

