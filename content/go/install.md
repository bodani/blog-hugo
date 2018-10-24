---
title: "Go 语言安装及配置"
date: 2018-10-23T14:10:27+08:00
draft: false
---

1.下载安装包

 https://golang.org/dl/  
 https://golang.google.cn/dl/

 将下载的二进制包解压至 /usr/local目录

2.配置环境变量

cat /etc/profile.d/go.sh 
```
export GOROOT=/usr/local/go
export GOPATH=~/golib:~/goproject
export GOBIN=~/gobin
export PATH=$PATH:$GOROOT/bin:$GOBIN
```

说明: 

GOROOT go安装包存放位置  
GOPATH 工作区，多个工作区之间用冒号间隔  
GOBIN  可执行文件目录  
PATH   系统环境变量 

3.目录结构

 src 源码  
 pkg 归档文件 .a 后缀  

4.常用命令及参数

go run  执行 

   -a -n - p -w

go build 

go install 编译并安装指定的代码包及它们的依赖包 　

go get 下载远程代码到GOPATH第一个工作区中，并编译执行i

go clean 

go doc 

go list 

go fmt 

go fix 

