---
title: "mdadm 软Raid 管理"
date: 2018-12-03T13:39:39+08:00
draft: false
---

#### 背景

mdadm是linux下用于创建和管理软件RAID的命令，是一个模式化命令。但由于现在服务器一般都带有RAID阵列卡，并且RAID阵列卡也很廉价，且由于软件RAID的自身缺陷（不能用作启动分区、使用CPU实现，降低CPU利用率），因此在生产环境下并不适用。但为了学习和了解RAID原理和管理，因此仍然进行一个详细的讲解：

#### 安装

```
yum install mdadm -y
```

#### 组建raid

```
组装raid 
mdadm -C /dev/md0 -a yes -n 4 -l 10 /dev/sdb /dev/sdc /dev/sdd /dev/sde

说明 : 
专用选项：
-l 级别
-n 设备个数
-a {yes|no} 自动为其创建设备文件
-c 指定数据块大小（chunk）
-x 指定空闲盘（热备磁盘）个数，空闲盘（热备磁盘）能在工作盘损坏后自动顶替
注意：创建阵列时，阵列所需磁盘数为-n参数和-x参数的个数和
```

```
查看状态, 组装进度等
mdadm -D /dev/md0

也可以通过mdstat查看状态
cat /proc/mdstat

Personalities : [raid10] 
md127 : active raid10 sdd[2] sda[3] sdb[0] sdc[1]
      999950336 blocks super 1.2 512K chunks 2 near-copies [4/4] [UUUU]
      bitmap: 1/8 pages [4KB], 65536KB chunk

unused devices: <none>
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



#### 管理

选项：-a(--add)，-d(--del),-r(--remove),-f(--fail)

##### 模拟损坏  
```
mdadm /dev/md1 -f /dev/sdb5
```
##### 移除损坏的磁盘  
```
mdadm /dev/md1 -r /dev/sdb5
```
##### 添加新的硬盘到已有阵列   
```
mdadm /dev/md1 -a /dev/sdb7     
注意:   
1 新增加的硬盘需要与原硬盘大小一致    
2 如果原有阵列缺少工作磁盘（如raid1只有一块在工作，raid5只有2块在工作），这时新增加的磁盘直接变为工作磁盘，如果原有阵列工作正常，则新增加的磁盘为热备磁盘。 
```
##### 停止阵列  
```
选项：-S = --stop
mdadm -S /dev/md1
```
##### 启动阵列   
```
选项：-R= --run
mdadm -R  /dev/md1
```

[详情](https://www.cnblogs.com/zhangeamon/p/6866429.html)

