---
title: "时间点恢复"
date: 2019-01-24T11:08:54+08:00
draft: false
---

##### PITR
Point-in-time recovery


https://blog.csdn.net/a964921988/article/details/84957241

https://github.com/digoal/blog/blob/master/201608/20160823_03.md

https://github.com/digoal/blog/blob/master/201608/20160823_04.md


```

recovery.conf

recovery_target_action= 'pause'  # promote ,shutdown

```

```
--- 打lable 
select pg_create_restore_point('my_daily_process_ended');


--- 恢复到指定的lable
recovery.conf

recovery_target_name = 'my_daily_process_ended'
```

