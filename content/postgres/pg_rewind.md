---
title: "pg_rewind"
date: 2019-01-30T10:16:17+08:00
draft: false
---
pg_rewind requires that the target server either has the wal_log_hints option enabled in postgresql.conf or data checksums enabled when the cluster was initialized with initdb. Neither of these are currently on by default. full_page_writes must also be set to on, but is enabled by default.

之前一直没有成功，因为没有设置 

wal_log_hints

https://github.com/digoal/blog/blob/master/201901/20190128_02.md
