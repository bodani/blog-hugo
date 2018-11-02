---
title: "Docker 本地网络"
date: 2018-11-02T16:34:06+08:00
draft: false
---

#### 基础命令概览

```
docker network --help

Usage:	docker network COMMAND

Manage networks

Commands:
  connect     Connect a container to a network
  create      Create a network
  disconnect  Disconnect a container from a network
  inspect     Display detailed information on one or more networks
  ls          List networks
  prune       Remove all unused networks
  rm          Remove one or more networks

```
#### 默认网络

```
docker network ls
NETWORK ID          NAME                DRIVER              SCOPE
0770a8275bff        bridge              bridge              local
b6617326f199        host                host                local
31c55ffcf0a8        none                null                local

```

创建容器时通过 --network=  指定网络类型

- none 这个网络下的容器除了 lo，没有其他任何网卡。 
- host 共享Host的网络栈，容器的网络配置与 host 完全一样。
 -- 优点效率高
 -- 不足由于没有隔离，与host资源容易冲突。
- bridge 默认的网络类型 

#### Bridge 网络

Linux提供了许多虚拟设备，这些虚拟设备有助于构建复杂的网络拓扑，满足各种网络需求。

- 网桥（bridge）
网桥是一个二层设备，工作在链路层，主要是根据MAC学习来转发数据到不同的port。 看做物理设备中的交换机 ，或vlan
```
# 创建网桥
brctl addbr br0
# 添加设备到网桥
brctl addif br0 eth1
# 查询网桥mac表
brctl showmacs br0
```

- veth
veth pair是一对虚拟网络设备，一端发送的数据会由另外一端接受，常用于不同的网络命名空间。
```
# 创建veth pair
ip link add veth0 type veth peer name veth1
# 将veth1放入另一个netns
ip link set veth1 netns newns
```
- TAP/TUN 
TAP/TUN设备是一种让用户态程序向内核协议栈注入数据的设备，TAP等同于一个以太网设备，工作在二层；而TUN则是一个虚拟点对点设备，工作在三层。
```
ip tuntap add tap0 mode tap
ip tuntap add tun0 mode tun
```

Docker 安装后默认有一个名称为docker0 的bridge

```
brctl show # yum install bridge-utils
bridge name	bridge id		STP enabled	interfaces
docker0		8000.024262081be1	no		veth1878bba
``` 




