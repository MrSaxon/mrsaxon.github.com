---
layout: post
title: "MAC_自动挂载samba"
description: ""
category: "MAC"
tags: ["MAC"]
---
{% include JB/setup %}

```ruby
$ sudo vim /etc/fstab
#添加以下内容，根据实际配置进行修改
10.81.5.20:/video /opt/samba url automounted,url==smb://user:pass123@10.81.5.20/samba 0 0
```

潜在风险
如果在无法连接samba的网络环境下容易导致mac启动失败
