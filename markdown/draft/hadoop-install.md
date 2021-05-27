---
title: Hadoop集群搭建
cover: 
date: 2021-04-27
tags: install spark hive
categories: hadoop
---

Hadoop2.0版本开始支持ProxyUser的机制。含义是使用User A的用户认证信息，以User B的名义去访问hadoop集群。



服务端需要在NameNode和ResourceManager的core-site.xml中进行代理权限相关配置。 对于每一个superUser用户，配置参数：

配置	说明
hadoop.proxyuser.$superuser.hosts	配置该superUser允许通过代理访问的主机节点
hadoop.proxyuser.$superuser.groups	配置该superUser允许代理的用户所属组
hadoop.proxyuser.$superuser.users	配置该superUser允许代理的用户
对于每个superUser用户，hosts必须进行配置，而groups和users至少需要配置一个。

这几个配置项的值都可以使用*来表示允许所有的主机/用户组/用户。

例如：

<property>
<name>hadoop.proxyuser.userA.hosts</name>
<value>*</value>
</property>
<property>

参考：
http://dblab.xmu.edu.cn/blog/install-hadoop-cluster/
https://blog.csdn.net/weixin_36836847/article/details/95510843


<name>hadoop.proxyuser.userA.users</name>
<value>user1,user2</value>
</property>
表示允许用户userA，在任意主机节点，代理用户user1和user2














# 安装过程出现的问题：

## mysql安装问题

```
软件包：mysql-community-server-5.7.20-1.el6.x86_64 (mysql57-community)

需要：libsasl2.so.2()(64bit)
```

解决：
centos7上sasl是so.3,centos6才是so.2

1. 换个更高版本的mysql
2. 换个centos6

## mysqld --initialize没有给出登录密码
打开/etc/my.cnf文件找到错误日志位置  
打开错误日志查找密码

## systemctl start mysqld启动mysql服务失败
检查mysql的data目录的文件所有者以及所属用户组是否为mysql（data目录地址可以在my.cnf中找到）

## schematool  -initSchema -dbType mysql错误
```
Exception in thread "main" java.lang.RuntimeException: com.ctc.wstx.exc.WstxParsingException: Illegal character entity: expansion character (code 0x8
 at [row,col,system-id]: [3215,96,"file:/usr/local/hive/conf/hive-site.xml"]
```

## 

Error: Error while compiling statement: FAILED: SemanticException Failed to get a spark session: org.apache.hadoop.hive.ql.metadata.HiveException: Failed to create Spark client for Spark session 1c66c552-f9c3-4880-8152-c97a900cb9ba (state=42000,code=40000)  
beeline 连接时应该使用-n指定用户

## spark on yarn无法正常运行
检查hostname
检测/etc/hosts