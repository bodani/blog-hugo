---
title: "Ansible Roles"
date: 2018-10-29T14:09:08+08:00
draft: false
---

以特定的层级目录结构进行组织的tasks、variables、handlers、templates、files等

mkdir -pv ./{os_hard,nginx,memcached}/{files,templates,vars,handlers,meta,default,tasks}/main.yaml

```
tree memcached/
memcached/
├── default 设定默认变量
│   └── main.yaml
├── files 存储由copy或script等模块调用的文件 
│   └── main.yaml
├── handlers 
│   └── main.yaml
├── meta 定义当前角色的特殊设定及其依赖关系
│   └── main.yaml
├── tasks
│   └── main.yaml
├── templates 存储由template模块调用的模板文本
│   └── main.yaml
└── vars
    └── main.yaml
```


[k8s](https://github.com/gjmzj/kubeasz)  
[TiDB](https://github.com/pingcap/tidb-ansible)   
[小试牛刀](https://github.com/bodani/ansible-moses)
