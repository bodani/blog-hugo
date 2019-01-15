---
title: "Docker 问题集"
date: 2019-01-03T15:06:43+08:00
draft: false
---

- Docker push: Received unexpected HTTP status: 500 Internal Server Error

描述: 使用jenkins 构建docker images时 push images到私有harbor中报错: Received unexpected HTTP status: 500 Internal Server Error,在build机上直接push没有问题。有的项目可以成功，有的失败。即使同一个项目有时执行成功，有时也会失败。

解决方式: 网上很多的关于500的错误，大都是关闭selinux来解决。但是情况与这个不同。现在的问题时在jenkins中执行有问题，直接裸机执行没有问题。

看到这篇[文章](https://www.jfrog.com/jira/browse/RTFACT-9025 )， 怀疑时在push images 过大时需要的系统内存不足导致。 

调整jenkins启动时java的内存参数 

```
JAVA_OPTS="-Djava.util.logging.config.file=/var/jenkins_home/log.properties -Duser.timezone=Asia/Shanghai  -Xms4096m -Xmx4096m 
```

问题过几天后有出现

```
/var/log/message 
kernel crash after "unregister_netdevice: waiting for lo to become free. Usage count = 
```

换台build机

问题解决
