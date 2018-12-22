---
title: "Postgres 监控"
date: 2018-12-06T16:21:08+08:00
draft: false
---

[zabbix](https://github.com/cavaliercoder/libzbxpgsql)

[postgres_exporter](https://github.com/wrouesnel/postgres_exporter)

[pgwatch2](https://github.com/cybertec-postgresql/pgwatch2)

[pgmetrics](https://github.com/rapidloop/pgmetrics)

[pgdash](https://pgdash.io/)

状态查看
[pgcenter](https://github.com/lesovsky/pgcenter)


```
pgcenter top
pgcenter: 2018-12-20 11:10:25, load average: 0.94, 0.84, 0.86                                                                         state [ok]: ::1:5432 postgres@postgres (ver: 10.6, up 8 days 19:57:54, recovery: f)
    %cpu: 15.0 us,  3.7 sy,  0.0 ni, 75.3 id,  5.7 wa,  0.0 hi,  0.2 si,  0.0 st                                                        activity:  5/1000 conns,  0/0 prepared,  2 idle,  0 idle_xact,  3 active,  0 waiting,  0 others
 MiB mem:   7821 total,    162 free,    424 used,     7235 buff/cached                                                                autovacuum:  0/3 workers/max,  0 manual,  0 wraparound, 00:00:00 vac_maxtime
MiB swap:   1023 total,    903 free,    120 used,      0/0 dirty/writeback                                                            statements: 1888 stmt/s, 2.330 stmt_avgtime, 00:00:00 xact_maxtime, 00:00:00 prep_maxtime      

pid     cl_addr      cl_port   datname       usename    appname    backend_type        wait_etype   wait_event     state    xact_age   query_age         change_age        query           
27908   ::1          40204     postgres      postgres   pgcenter   client backend                                  active   00:00:00   00:00:00          00:00:00          SELECT pid, client_addr AS cl_addr, client_port AS cl_port, datname, usename, left(application
27660   10.1.88.22   34224     timescaledb   postgres              client backend      LWLock       WALWriteLock   active   00:00:00   00:00:00          00:00:00          COMMIT                                                                                        
27410   10.1.88.22   34058     timescaledb   postgres              client backend                                  active   00:00:00   00:00:00          00:00:00          COMMIT                 
```

[monitoring-stats](https://www.postgresql.org/docs/devel/monitoring-stats.html)


