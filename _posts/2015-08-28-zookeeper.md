---
layout: post
title: "Zookeeper集群搭建"
description: ""
category: "Zookeeper"
tags: ["Zookeeper"]
---
{% include JB/setup %}

### Zookeeper简介

ZooKeeper 是一个为分布式应用所设计的分布的、开源的协调服务。分布式的应用可以建立在同步、配置管理、分组和命名等服务的更高级别的实现的基础之上。 ZooKeeper 意欲设计一个易于编程的环境，它的文件系统使用我们所熟悉的目录树结构。 ZooKeeper 使用 Java 所编写，但是支持 Java 和 C 两种编程语言。    
众所周知，协调服务非常容易出错，但是却很难恢复正常，例如，协调服务很容易处于竞态以至于出现死锁。我们设计 ZooKeeper 的目的是为了减轻分布式应用程序所承担的协调任务。

### Zookeeper下载地址   

```ruby
http://zookeeper.apache.org/releases.html   
```

本人下载最新稳定版本zookeeper-3.4.6，解压至/home/opt/zookeeper-3.4.6   

### 设置环境变量   

```ruby
#Set ZooKeeper Enviroment   
export ZOOKEEPER_HOME=/home/opt/zookeeper-3.4.6    
export PATH=$PATH:$ZOOKEEPER_HOME/bin:$ZOOKEEPER_HOME/conf   
```

### 修改配置文件

Zookeeper的模式分为三种：单机模式、集群模式、伪集群模式   

单机模式

```ruby
cd zookeeper目录/conf   
cp zoo_sample.cfg zoo.cfg    
vim zoo.cfg   
修改dataDir=/home/opt/zookeeper，一个初始为空的目录，根据自己的部署进行设置   

tickTime ：基本事件单元，以毫秒为单位。它用来指示心跳，最小的 session 过期时间为两倍的tickTime   
dataDir ：存储内存中数据库快照的位置，如果不设置参数，更新事务日志将被存储到默认位置   
clientPort ：监听客户端连接的端口   
```

为了获得可靠的 ZooKeeper 服务，用户应该在一个集群上部署 ZooKeeper 。只要集群上大多数的ZooKeeper 服务启动了，那么总的 ZooKeeper 服务将是可用的。另外，最好使用奇数台机器。 如果 zookeeper拥有 5 台机器，那么它就能处理 2 台机器的故障了。

集群模式在配置文件下增加    

```ruby
server.1=IP1:2888:3888   
server.2=IP2:2888:3888   
server.3=IP3:2888:3888   

对应的IP为每台集群节点的IP地址，id为每台节点在集群中的唯一标示，可选范围为1-255
```

伪集群模式

```ruby
需要在conf下建立不同的配置文件，以三个节点为例，需要创建zoo1.cfg,zoo2.cfg,zoo3.cfg三个配置文件，每个配置文件内的dataDir要区分配置，每个配置文件的server配置保持一致即可
```

### 服务运行

```ruby
单机模式启动：   
cd /home/opt/zookeeper-3.4.6/bin   
./zkServer.sh start   

可以在当前目录中查看zookeeper.out查看启动输出日志   

集群模式启动：   
集群的节点启动顺序与zoo.cfg的配置文件内server-id顺序保持一致
每个节点的启动方法同上一致    

伪集群模式：    
./zkServer.sh start zoo1.cfg   
./zkServer.sh start zoo2.cfg   
./zkServer.sh start zoo3.cfg   

通过./zkServer.sh status可以查看服务运行状态   
JMX enabled by default   
Using config: /home/opt/zookeeper-3.4.6/bin/../conf/zoo.cfg   
Mode: follower   

可以通过Mode查看当前节点在zookeeper集群的角色是leader还是follower
```

### 错误汇总

问题一：Either no config or no quorum defined in config, running  in standalone mode    
对应节点端口没有启动，一般是配置文件存在问题，需要检查保持所有集群节点配置文件一致   

问题二：Cannot open channel to 3 at election address xxx/xxx:3888    
启动第一个节点服务的时候会报出以下错误，不要紧张，这个是因为后续的节点服务没有启动，启动后会自然消失   
