---
title: "终端复用"
date: 2018-12-25T10:06:42+08:00
draft: false
---

#### 背景

我们在linux服务器上的工作一般都是通过一个远程的终端连接软件连接到远端系统进行操作，例如使用xshell或者SecureCRT工具通过ssh进行远程连接。
在使用过程中，如果要做比较耗时的操作，例如有时候进行编译，或者下载大文件需要比较长的时间，一般情况下是下班之后直接运行希望第二天早上过来运行完成，这样就不用耽误工作时间。
但是网络有时候不稳定，可能在半夜会出现连接断掉的情况，一旦连接断掉，我们所执行的程序也就中断，我们当然可以写一个脚本后台运行，但是还是不方便。那么有没有一种工具可以解决这样的问题呢。

- tmux
- gux
- screem


#### 详解 [tmux](https://www.cnblogs.com/wangqiguo/p/8905081.html)


