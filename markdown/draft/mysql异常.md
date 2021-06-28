---
title: Mysql异常解决方案
cover: 
date: 2021-06-28
tags: 
categories: 

---

# [08s01] Communications link failure

## 问题描述
hibernate连接mysql执行sql语句时，有时成功有时失败。

## 异常信息
```
[org.hibernate.engine.jdbc.spi.SqlExceptionHelper]SQL Error: 0, SQLState: 08S01
[org.hibernate.engine.jdbc.spi.SqlExceptionHelper]Communications link failure

The last packet successfully received from the server was 18,687 milliseconds ago.  The last packet sent successfully to the server was 0 milliseconds ago.
[org.hibernate.engine.jdbc.spi.SqlExceptionHelper]SQL Error: 0, SQLState: null
[org.hibernate.engine.jdbc.spi.SqlExceptionHelper]connection is closed
[org.jeecgframework.core.common.exception.GlobalExceptionResolver]全局处理异常捕获:
java.sql.SQLException: connection is closed
        at com.alibaba.druid.pool.DruidPooledConnection.checkState(DruidPooledConnection.java:1050)
        at com.alibaba.druid.pool.DruidPooledConnection.getAutoCommit(DruidPooledConnection.java:722)
        at org.hibernate.engine.jdbc.internal.LogicalConnectionImpl.isAutoCommit(LogicalConnectionImpl.java:392)
        at org.hibernate.engine.transaction.internal.TransactionCoordinatorImpl.afterNonTransactionalQuery(TransactionCoordinatorImpl.java:195)
        at org.hibernate.internal.SessionImpl.afterOperation(SessionImpl.java:511)
        at org.hibernate.internal.SessionImpl.list(SessionImpl.java:1166)
        at org.hibernate.internal.QueryImpl.list(QueryImpl.java:101)
	...
```
## 解决方案
1. 检查mysql服务器磁盘容量是否不足（好家伙，没有第一时间检查，白忙活了一天）。
2. 如果服务器容量没到99%，则可以尝试修改mysql服务器wait_timeout参数。