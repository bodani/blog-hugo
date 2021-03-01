---
title: "PG高可用Patroni"
date: 2019-01-30T10:14:55+08:00
draft: false
---

#### 实现目标

- 高可用方案对比
- patroni 架构分析
- patroni 搭建新集群
- patroni 接管现有集群
- patroni 管理pg配置
- 手动swithover
- 自动failover
- 维护模式
- 弹性扩容，缩容
- 对外提供统一服务
- 备份恢复
- 监控
- 日志
- 升级



[参考]

https://www.cnblogs.com/zhangeamon/p/9772118.html

https://www.linode.com/docs/databases/postgresql/create-a-highly-available-postgresql-cluster-using-patroni-and-haproxy

[使用维护手册](./book/patroni使用维护手册.pdf)

[ansible 管理](https://github.com/vitabaks/postgresql_cluster)

[实践](https://mp.weixin.qq.com/s/edvWkTb-WF7YyVAFz5GCfw)

其他HA 方案

[repmgr](https://github.com/2ndQuadrant/repmgr)
[PAF](https://github.com/clusterlabs/PAF/)
