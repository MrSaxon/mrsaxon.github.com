---
layout: post
title: "Mac下Parallels Win7激活失败解决方案"
description: ""
category: "随笔"
tags: ["Mac Parallels", "Win7"]
---
{% include JB/setup %}

本人用过一段时间VMware（在买Mac时候老板直接给拷贝过来了），经过同事推荐得知Parallels虚拟机，于是下载了一个新的win7镜像，重新安装一个虚拟机尝试下，我采用了Parallels的融合模式安装，Mac和虚拟机会自动分析软件运行方式（本机Or虚拟机），个人感觉真心不错！另外虚拟机下的激活应用可以直接显示在dock上，不在耽误win应用关注度，赞一个！另外本人还进行了一番游戏测试，平时比较爱玩Dota和英雄联盟，两个游戏在虚拟机下进行测试，帧数30+，比较流畅，但是有两个问题，玩dota会感觉鼠标缓慢迟钝，我们可以在虚拟机的菜单栏上点击设备，然后选择usb，找到你的usb鼠标，点击，会提示你如果Yes的话只能在虚拟机下使用鼠标，确定后会发现鼠标的dpi直接达到了自己的预期效果，哈哈！对于英雄联盟的话，如果只是针对windows使用鼠标的话会有偶尔鼠标图像丢失的情况，由于暂时没有研究原因，建议大家就不要只针对windows下使用了，移动速度也够玩的了~

其实我们自己装玩系统后必做的一件事就是激活我们的操作系统，我采用了一款win7激活工具，然后按照提示进行操作，最后重启计算机，waiting…

“No Free Memory for XSDT”系统没有办法启动了，报了这个错误，这尼玛坑了，我按照要求按随意一个按键启动系统，然后发现系统激活失败！fuck

这个时候伟大的搜索引擎帝降临到我的身边，遇到这个问题的不止我一个啊，爽了！

<a href="http://kb.parallels.com/Attachments/15646/Images/boot%20flags.png" target="_blank">http://kb.parallels.com/en/111406</a>

<p>Cause</p>
<p>Slic loader application that was previously installed on your Windows system doesn’t support WAET functionality.
WAET stands for Windows ACPI Emulated Device Table. It is an optimization tool for virtualized guest OS from Microsoft.
WAET support will be introduced in future version of Parallels Desktop for Mac.</p>
<p>Resolution</p>
<p>Meanwhile you can disable WAET support in Parallels Desktop for Mac:</p>

1. Go to Parallels Desktop Configuration –> Hardware –> Boot Order and find Boot Flags field:

<img src="http://kb.parallels.com/Attachments/15646/Images/boot%20flags.png" />

2. Copy/paste the following line there:
kernel.waet.enable=0

3. Save the changes and start your virtual machine.
