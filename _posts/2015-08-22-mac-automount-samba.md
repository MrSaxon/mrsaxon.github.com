---
layout: post
title: "MAC_自动挂载samba"
description: ""
category: "MAC"
tags: ["MAC"]
---
{% include JB/setup %}

{% highlight php %}
sudo vim /etc/fstab

10.81.5.20:/video /opt/samba url automounted,url==smb://user:pass123@10.81.5.20/samba 0 0
{% endhighlight %}
