---
layout: post
title: "mcrypt安装遇到configure: error: “You need at least libmhash 0.8.15 to compile this program. http://mhash.sf.net/”"
description: ""
category: "PHP扩展安装"
tags: ["mcrypt", "mhash版本低"]
---
{% include JB/setup %}

当安装mcrypt遇到以上错误时候，可以使用以下方式进行解决

{% highlight php %}
export LD_LIBRARY_PATH=/usr/local/lib
export LDFLAGS="-L/user/local/lib/ -I/usr/local/include/"
export CFLAGS="-I/usr/local/include/"
{% endhighlight %}
