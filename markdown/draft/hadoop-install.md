---
title: 
cover: 
date: 2021-04-27
tags: install
categories: hadoop
---

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
<name>hadoop.proxyuser.userA.users</name>
<value>user1,user2</value>
</property>
表示允许用户userA，在任意主机节点，代理用户user1和user2