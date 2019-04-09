---
title: "nginx"
date: 2019-04-09T15:42:15+08:00
draft: false
---

[性能优化](https://mp.weixin.qq.com/s/YoZDzY4Tmj8HpQkSgnZLvA)

- 错误码 502   ， error.log 中错误信息 [error] 236#236: *8371899 upstream sent too big header while reading response header from upstream,

问题 header 过大
```
proxy_buffer_size 64k;
proxy_buffers   4 32k;
proxy_busy_buffers_size 64k;
```
[官网说明](http://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_buffer_size)
