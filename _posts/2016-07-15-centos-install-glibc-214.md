---
layout: post
title: "Linux_Centos升级glibc版本"
description: ""
category: "Linux"
tags: ["Linux", "Centos", "glibc"]
---
{% include JB/setup %}

根据项目上的需要，由于现有的操作系统glibc为2.12版本，需要升级至2.14版本，在此记录升级过程

### 操作系统版本
```sh
# cat /etc/issue
CentOS release 6.3 (Final)
Kernel \r on an \m

```

### glibc软件列表
* glibc-2.14 [下载](http://ftp.gnu.org/gnu/glibc/glibc-2.14.tar.gz)
* glibc-linuxthreads-2.5 [下载](http://ftp.gnu.org/gnu/glibc/glibc-linuxthreads-2.5.tar.bz2)

### 升级过程
```sh
切换用户至root，不切换也可以，最后make install时候需要sudo权限

# mkdir glibc-build && cd glibc-build
# 放入下载好的软件列表或者不提前下载在此步骤进行wget
# tar zxf glibc-2.14.tar.gz
# cd glibc-2.14
# tar jxf ../glibc-linuxthreads-2.5.tar.bz2
# cd ../
# ./glibc-2.14/configure --prefix=/usr --disable-profile --enable-add-ons --with-headers=/usr/include --with-binutils=/usr/bin
# make
# make install    (非root用户请使用 sudo make install, 并确保个人用户有sudo权限)
```

### 检验是否成功
```sh
# ls -l /lib64/libc.so.6
lrwxrwxrwx 1 root root 12 7月  15 14:47 /lib64/libc.so.6 -> libc-2.14.so*
```

### 安装过程中遇到的问题
#### 问题一：/usr/bin/ld报错
/usr/bin/ld: cannot find -lnss_test1
collect2: ld returned 1 exit status
Execution of gcc -B/usr/bin/ failed!
可以忽略

#### 问题二：关于/lib64/libc.so.6
必须确保/lib64/libc.so.6指向一个有效libc的so文件，否则会直接引起系统问题，需要重装系统

#### 问题三：关于locale
软件列表中有两个，请确保两个都进行安装，否则会导致locale报错, locale: Cannot set LC_CTYPE to default locale: No such file or directory
