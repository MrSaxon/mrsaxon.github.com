---
layout: post
title: "jekyll gem 安装报错"
description: ""
category: "jekyll"
tags: ["jekyll"]
---

{% highlight ruby %}   
$ gem install jekyll   
ERROR:  Could not find a valid gem 'jekyll' (>= 0), here is why:   
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Unable to download data from https://rubygems.org/ - Errno::ECONNRESET: Connection reset by peer - SSL_connect (https://rubygems.org/latest_specs.4.8.gz)   
{% endhighlight %}   

解决办法
{% highlight ruby %}   
$ gem sources --remove https://rubygems.org/   
$ gem sources -a https://ruby.taobao.org/   
$ sudo gem update --system   
$ sudo gem install jekyll   
{% endhighlight %}   