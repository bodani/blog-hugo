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

```
如下信息说明： 提示软raid 不能作为启动分区
mdadm: Note: this array has metadata at the start and
    may not be suitable as a boot device.  If you plan to
    store '/boot' on this device please ensure that
    your boot-loader understands md/v1.x metadata, or use
    --metadata=0.90
Continue creating array? y
```

[详情](https://www.cnblogs.com/zhangeamon/p/6866429.html)

