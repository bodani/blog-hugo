---
title: "docker 管理"
date: 2019-03-18T08:58:07+08:00
draft: false
---

- 清除不在需要的资源

```
This will remove:
        - all stopped containers
        - all networks not used by at least one container
        - all dangling images
        - all build cach

# docker system prune  -f
```
