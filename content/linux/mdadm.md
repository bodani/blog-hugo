---
title: "mdadm 软Raid 管理"
date: 2018-12-03T13:39:39+08:00
draft: false
---

```
yum install mdadm -y

mdadm -C /dev/md0 -a yes -n 4 -l 10 /dev/sdb /dev/sdc /dev/sdd /dev/sde

mdadm -D /dev/md0

```

[详情](https://www.cnblogs.com/zhangeamon/p/6866429.html)

