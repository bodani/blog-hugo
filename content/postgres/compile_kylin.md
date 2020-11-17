---
title: "kylin系统postgresql编译安装"
date: 2020-11-16T15:26:59+08:00
draft: false
---

#### 银河麒麟V10编译安装postgresql12.5

###### 安装openssl
麒麟v10 版操作系统openssl 被指定义安装在内核中。在安装postgresql时支持openssl编译不能通过。
解决思路，独立安装openssl,postgres对ssl 的依赖指向独立安装的openssl

```
查看原有版本
openssl version

下载并安装对应版本的openssl
wget https://www.openssl.org/source/openssl-1.1.1d.tar.gz
tar -zxf openssl-1.1.1d.tar.gz 
cd openssl-1.1.1d/
./config --prefix=/usr/local/openssl no-zlib 
```

###### 依赖包安装
```
yum install openldap-devel
yum install systemd-devel -y

```

###### 安装postgres

```
tar -zxf postgresql-12.5.tar.gz 

指定openssl 路径
./configure --with-openssl --with-includes=/usr/local/openssl/include/openssl --with-libraries=/usr/local/openssl/lib/ --with-systemd

./configure '--enable-rpath' '--prefix=/usr/pgsql-12' '--includedir=/usr/pgsql-12/include' '--libdir=/usr/pgsql-12/lib' '--mandir=/usr/pgsql-12/share/man' '--datadir=/usr/pgsql-12/share' '--with-icu' '--with-llvm' '--with-perl' '--with-python' '--with-tcl' '--with-tclconfig=/usr/lib64' '--with-openssl' '--with-pam' '--with-gssapi' '--with-includes=/usr/include:/usr/local/openssl/include/openssl' '--with-libraries=/usr/lib64:/usr/local/openssl/lib' '--enable-nls' '--enable-dtrace' '--with-uuid=e2fs' '--with-libxml' '--with-libxslt' '--with-ldap' '--with-selinux' '--with-systemd' '--with-system-tzdata=/usr/share/zoneinfo' '--sysconfdir=/etc/sysconfig/pgsql' '--docdir=/usr/pgsql-12/doc' '--htmldir=/usr/pgsql-12/doc/html' 'CFLAGS=-O2 -g -pipe -Wall -Wp,-D_FORTIFY_SOURCE=2 -fexceptions -fstack-protector-strong --param=ssp-buffer-size=4 -grecord-gcc-switches -m64 -mtune=generic' 'LDFLAGS=-Wl,--as-needed' 'LLVM_CONFIG=/usr/lib64/llvm5.0/bin/llvm-config' 'CLANG=/opt/rh/llvm-toolset-7/root/usr/bin/clang' 'PKG_CONFIG_PATH=:/usr/lib64/pkgconfig:/usr/share/pkgconfig' 'PYTHON=/usr/bin/python2'

./configure '--enable-rpath' '--prefix=/usr/pgsql-12' '--includedir=/usr/pgsql-12/include' '--libdir=/usr/pgsql-12/lib' '--mandir=/usr/pgsql-12/share/man' '--datadir=/usr/pgsql-12/share'  '--with-perl' '--with-python' '--with-openssl' '--with-pam' '--with-gssapi' '--with-includes=/usr/include:/usr/local/openssl/include/openssl' '--with-libraries=/usr/lib64:/usr/local/openssl/lib' '--enable-nls' '--enable-dtrace' '--with-uuid=e2fs' '--with-ldap' '--with-selinux' '--with-systemd' '--with-system-tzdata=/usr/share/zoneinfo' 
```

##### 安装postgis
```
http://postgis.net/source/  
```
