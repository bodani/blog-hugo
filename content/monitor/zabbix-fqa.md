---
title: "Zabbix FQA"
date: 2018-12-24T16:53:20+08:00
draft: false
---

#### FQA

- Too Many Process

原因: 被监控的主机进程数过多  
分析: 

```
获取当前进程数 ， 127.0.0.1 改为对应节点IP
zabbix_get -s 127.0.0.1 -k 'proc.num[]'  

查询是哪个应用占有的,如查看zabbix-agent占有进程数量
zabbix_get -s 127.0.0.1 -k 'proc.num[zabbix]'

比较有效的方法,将zabbix 换成想要查看的应用

ps -ef | wc
ps -ef | grep zabbix | wc  

```

[更多](https://www.zabbix.com/documentation/4.0/zh/manual/appendix/items/proc_mem_num_notes?s[]=proc&s[]=num)



