---
title: "Centos 初始化配置"
date: 2018-12-03T10:34:06+08:00
draft: false
---

#### 网络配置 

##### 设置IP 
 略
##### ip_froward  
查看 sysctl -a | grep ip_    
修改 vi /etc/sysctl.conf   
```
net.ipv4.ip_forward = 1
```

##### 最大使用内存
```
vm.max_map_count=262144
```
生效 sysctl -p

#### 系统更新

```
yum -y update
```

#### 安装扩展及工具

```
yum -y install epel-release net-tools bind-utils
```

#### 文件链接限制

查看 

```
ulimit -n
```

修改 
vi /etc/security/limits.conf  
```
* - nofile 65536
* soft nproc 65536
* hard nproc 65536
* soft nofile 65536
* hard nofile 65536
```
```
rm /etc/security/limits.d/*  -rf
```

#### 安全

##### selinux

查看 

```
getenforce
```
    
修改 临时 setenforce 0 
永久 vi /etc/sysconfig/selinux

##### 设置 firewalld

##### 设置 ssh 

禁用root用户，密钥登陆 切换为其他用户登录

```
PermitRootLogin without-password
#PermitRootLogin yes
```

修改22端口

登陆加速 
```
GSSAPIAuthentication no
UseDNS no
```
#### 系统时间

ntp
```
yum install chrony
systemctl enable chronyd.service
systemctl start chronyd.service
``` 

建议使用 yum install ntp
```
systemctl start ntpd
systemctl enable ntpd
```
