---
layout: post
title: "mcrypt安装遇到at least libmhash 0.8.15解决办法"
description: ""
category: "PHP扩展安装"
tags: ["mcrypt", "mhash版本低"]
---
{% include JB/setup %}
当mcrypt安装遇到

```ruby
configure: error: “You need at least libmhash 0.8.15 to compile this program. http://mhash.sf.net/”
```

错误时候，可以使用以下方式进行解决

```ruby
$ export LD_LIBRARY_PATH=/usr/local/lib
$ export LDFLAGS="-L/user/local/lib/ -I/usr/local/include/"
$ export CFLAGS="-I/usr/local/include/"
```
