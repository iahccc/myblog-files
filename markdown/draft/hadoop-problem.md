---
title: hadoop集群问题
cover: 
date: 2021-05-28
tags: problem
categories: hadoop

---

# spark on yarn出现时而正常时而异常的现象
## 问题描述
使用spark-shell --master yarn，有时会在等待正常的时间后进入shell，有时则会在等待漫长的时间之后还是卡住，无法进入。
使用hive on spark也是出现类似现象，有时语句能正常执行，有时就会出现`org.apache.hadoop.hive.ql.metadata.HiveException: Failed to create Spark client for Spark session`。网上查到的解决方法大多是增加hive-site.xml中相关的timeout参数来解决，可是增加之后不但没有解决，卡住的时间还更长了...  
于是开始查找日志，在resourcemanage的日志中找到如下异常
```
2021-05-26 15:59:00,672 INFO org.apache.hadoop.yarn.server.resourcemanager.amlauncher.AMLauncher: Error launching appattempt_1622058364603_0002_000001. Got exception: java.net.ConnectException: Call From localhost/127.0.0.1 to localhost:39459 failed on connection exception: java.net.ConnectException: Connection refused; For more details see:  http://wiki.apache.org/hadoop/ConnectionRefused
        at sun.reflect.GeneratedConstructorAccessor61.newInstance(Unknown Source)
        at sun.reflect.DelegatingConstructorAccessorImpl.newInstance(DelegatingConstructorAccessorImpl.java:45)
        at java.lang.reflect.Constructor.newInstance(Constructor.java:423)
        at org.apache.hadoop.net.NetUtils.wrapWithMessage(NetUtils.java:836)
        at org.apache.hadoop.net.NetUtils.wrapException(NetUtils.java:760)
        at org.apache.hadoop.ipc.Client.getRpcResponse(Client.java:1566)
        at org.apache.hadoop.ipc.Client.call(Client.java:1508)
        at org.apache.hadoop.ipc.Client.call(Client.java:1405)
        at org.apache.hadoop.ipc.ProtobufRpcEngine$Invoker.invoke(ProtobufRpcEngine.java:233)
        at org.apache.hadoop.ipc.ProtobufRpcEngine$Invoker.invoke(ProtobufRpcEngine.java:118)
        at com.sun.proxy.$Proxy83.startContainers(Unknown Source)
        at org.apache.hadoop.yarn.api.impl.pb.client.ContainerManagementProtocolPBClientImpl.startContainers(ContainerManagementProtocolPBClientImpl.java:128)
        at sun.reflect.GeneratedMethodAccessor39.invoke(Unknown Source)
        at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)
        at java.lang.reflect.Method.invoke(Method.java:498)
        at org.apache.hadoop.io.retry.RetryInvocationHandler.invokeMethod(RetryInvocationHandler.java:422)
        at org.apache.hadoop.io.retry.RetryInvocationHandler$Call.invokeMethod(RetryInvocationHandler.java:165)
        at org.apache.hadoop.io.retry.RetryInvocationHandler$Call.invoke(RetryInvocationHandler.java:157)
        at org.apache.hadoop.io.retry.RetryInvocationHandler$Call.invokeOnce(RetryInvocationHandler.java:95)
        at org.apache.hadoop.io.retry.RetryInvocationHandler.invoke(RetryInvocationHandler.java:359)
        at com.sun.proxy.$Proxy84.startContainers(Unknown Source)
        at org.apache.hadoop.yarn.server.resourcemanager.amlauncher.AMLauncher.launch(AMLauncher.java:127)
        at org.apache.hadoop.yarn.server.resourcemanager.amlauncher.AMLauncher.run(AMLauncher.java:312)
        at java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1149)
        at java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:624)
        at java.lang.Thread.run(Thread.java:748)
Caused by: java.net.ConnectException: Connection refused
        at sun.nio.ch.SocketChannelImpl.checkConnect(Native Method)
        at sun.nio.ch.SocketChannelImpl.finishConnect(SocketChannelImpl.java:715)
        at org.apache.hadoop.net.SocketIOWithTimeout.connect(SocketIOWithTimeout.java:206)
        at org.apache.hadoop.net.NetUtils.connect(NetUtils.java:534)
        at org.apache.hadoop.ipc.Client$Connection.setupConnection(Client.java:699)
        at org.apache.hadoop.ipc.Client$Connection.setupIOstreams(Client.java:812)
        at org.apache.hadoop.ipc.Client$Connection.access$3800(Client.java:413)
        at org.apache.hadoop.ipc.Client.getConnection(Client.java:1636)
        at org.apache.hadoop.ipc.Client.call(Client.java:1452)

```

看了下本机确定没有开放39459端口，后来重启几次发现这个端口号还会变...，哪应该就是动态的了。  
经过不断查找日志发现如下关键信息
```
2021-05-26 15:46:06,939 INFO org.apache.hadoop.yarn.server.resourcemanager.rmnode.RMNodeImpl: localhost:39459 Node Transitioned from NEW to RUNNING
2021-05-26 15:46:06,943 INFO org.apache.hadoop.yarn.server.resourcemanager.scheduler.fair.FairScheduler: Added node localhost:39459 cluster capacity: <memory:8192, vCores:8>
2021-05-26 15:46:10,833 INFO org.apache.hadoop.yarn.server.resourcemanager.ResourceTrackerService: NodeManager from node localhost(cmPort: 44387 httpPort: 8042) registered with capability: <memory:8192, vCores:8>, assigned nodeId localhost:44387
2021-05-26 15:46:10,833 INFO org.apache.hadoop.yarn.server.resourcemanager.rmnode.RMNodeImpl: localhost:44387 Node Transitioned from NEW to RUNNING
2021-05-26 15:46:10,834 INFO org.apache.hadoop.yarn.server.resourcemanager.scheduler.fair.FairScheduler: Added node localhost:44387 cluster capacity: <memory:16384, vCores:16>
```
原来这是NodeManage的端口信息，从日志里看有两条`Added node`记录，因为我的集群确实只有两个节点，但问题是两个节点为什么都是localhost？这就是问题所在了。在两个机器上分别查找这两个端口确实是存在的。  

## 问题解决
搜索资料发现搭建hadoop的过程中不同机器需要修改hostname，查了一下两台机器的hostname确实都是localhost，于是把两台机器修改为不同的hostname（到这一步你们应该就已经可以解决问题了，如果还不行就往下看）。  
我改完了之后以为就万事大吉了，日志里`Added node`记录还是显示的是localhost。。。   

后面找了很久才发现问题出现在`/etc/hosts`文件。  
案发现场：
```
127.0.0.1   localhost localhost.localdomain localhost4 localhost4.localdomain4
::1         localhost localhost.localdomain localhost6 localhost6.localdomain6

192.168.28.100 localhost

192.168.28.100 master
192.168.28.101 slave
```
这是我某一台机器的hosts文件内容，这你能发现有什么问题吗？至少我不能...  
后面我阴差阳错的把`192.168.28.100 localhost`放到的文件最后，发现问题解决了...你也可以把该条记录删了。集群里的所有机器都要改。  
可能yarn是使用了当前ip对应的域名？