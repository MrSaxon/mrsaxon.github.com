---
layout: post
title: "php-zookeeper扩展安装"
description: ""
category: "Zookeeper"
tags: ["Zookeeper"]
---
{% include JB/setup %}

###扩展下载地址
https://github.com/andreiz/php-zookeeper

###安装
php-zookeeper扩展需要依赖libzookeeper，翻阅zookeeper官方文档
[传送门](http://zookeeper.apache.org/doc/r3.1.2/zookeeperProgrammers.html) ，我们下载对应zookeeper集群版本一致的软件包，进行解压缩，利用源码中c语言版本源码进行lib安装

```sh
cd [zookeeper tarball dir]/src/c   
./configure   
make && make install   

安装后可以在/usr/local/include/zookeeper中找到对应的header文件   
```

下面继续php-zookeeper扩展的安装   

```sh
cd [php-zookeeper-master]   
phpize   
./configure --with-php-config=[php-config-dir] --with-libzookeeper-dir=/usr/local   
注意上面的libzookeeper-dir的填写，只要写/usr/local即可，config会自动寻找这个目录下的include/zookeeper目录   

if test -r "$PHP_LIBZOOKEEPER_DIR/include/c-client-src/zookeeper.h"; then   
&nbsp;&nbsp;&nbsp;&nbsp;PHP_LIBZOOKEEPER_DIR="$PHP_LIBZOOKEEPER_DIR"   
elif test -r "$PHP_LIBZOOKEEPER_DIR/include/zookeeper/   zookeeper.h"; then   
&nbsp;&nbsp;&nbsp;&nbsp;PHP_LIBZOOKEEPER_DIR="$PHP_LIBZOOKEEPER_DIR"   
elif test -r "$PHP_LIBZOOKEEPER_DIR/include/zookeeper.h"; then   
&nbsp;&nbsp;&nbsp;&nbsp;PHP_LIBZOOKEEPER_DIR="$PHP_LIBZOOKEEPER_DIR"   
else   
&nbsp;&nbsp;&nbsp;&nbsp;AC_MSG_ERROR([Can't find zookeeper headers under "$PHP_LIBZOOKEEPER_DIR"])   
fi   

继续扩展安装，conigure完毕后   
make && make install   
修改php.ini将扩展加入进来   
[zookeeper]   
extension=zookeeper.so   
```

扩展使用参考：[示例](https://github.com/andreiz/php-zookeeper/blob/master/examples/Zookeeper_Example.php)
