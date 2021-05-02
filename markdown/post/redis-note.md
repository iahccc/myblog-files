---
title: Redis笔记
cover: 'https://cdn.jsdelivr.net/gh/iahccc/PicBed/img/archive_img.jpeg'
date: 2020-11-15 17:14:11
tags: redis
categories: redis
---

# Redis编译安装
## 安装步骤
    wget *redis安装包url*  

    tar -zxvf *redis安装包路径*  

    cd *redis解压包路径*  

    sudo mkdir /usr/local/redis  

    sudo make  

    sudo make PREFIX=/usr/local/redis install  

    sudo mkdir /usr/local/redis/etc  

    sudo cp redis.conf /usr/local/redis/etc/  

## 设置redis以守护进程方式运行
将/usr/local/redis/etc/redis.conf文件中的daemonize改为yes
## 添加环境变量
    # 如果提示权限不足,则先运行 su 命令进入管理员模式，再输入该语句。
    sudo echo 'export PATH="$PATH:/usr/local/redis/bin"' >> /etc/profile   
    source /etc/profile

## Redis高可用方案
Redis高可用常见的几种方式：
* Redis复制(Replication)和哨兵(Sentinel)机制
* Redis集群(Redis-cluster模式)

### Redis复制(Replication)和哨兵(Sentinel)机制
#### Replication工作原理
*todo*

#### Sentinel工作原理
*todo*

#### 搭建步骤
将redis.conf文件复制为redis-master-7000.conf、redis-slave-7001.conf、redis-slave-7002.conf并修改(文件名以端口号结尾)

    # 服务器端口号，主从分别修改为7000/7001/7002
    port <端口号>
    # 使得Redis可以跨网络访问 
    bind 0.0.0.0 
    # 配置reids的密码 
    requirepass "111111" 

    # 下面两个配置只需要配置从节点(slave) 
    # 配置主服务器地址、端口号 
    replicaof 127.0.0.1 7001 
    # 主服务器密码 
    masterauth "111111"

分别启动三个Redis服务

    redis-server /usr/local/redis/etc/redis-master-7000.conf
    redis-server /usr/local/redis/etc/redis-slave-7001.conf
    redis-server /usr/local/redis/etc/redis-slave-7002.conf

使用redis-cli工具连接redis服务查看主从节点是否搭建成功

    redis-cli -h <主机名> -p <端口号>
    
验证密码后输入进行验证
    info replication


接下来配置哨兵Sentinel。

将sentinel.conf文件复制为sentinel-27000.conf、sentinel-27001.conf、sentinel-27002.conf(文件名以端口号结尾)并修改 **(注意文件权限问题，需要给用户写权限)**

    # 三个配置文件分别配置不同的端口号
    port <端口号>
    # 设置为后台启动
    daemonize yes
    # 主节点的名称(可以自定义，与后面的配置保持一致即可)
    # 主机地址
    # 端口号
    # 数量(2代表只有两个或两个以上的哨兵认为主服务器不可用的时候，才会进行failover操作)
    sentinel monitor mymaster 127.0.0.1 6379 2
    # 多长时间没有响应认为主观下线(SDOWN)
    sentinel down-after-milliseconds mymaster 60000
    # 表示如果15秒后,mysater仍没活过来，则启动failover，从剩下从节点序曲新的主节点
    sentinel failover-timeout mymaster 15000
    # 指定了在执行故障转移时， 最多可以有多少个从服务器同时对新的主服务器进行同步， 这个数字越小， 完成故障转移所需的时间就越长
    sentinel parallel-syncs mymaster 1

启动三个Sentinel

    redis-sentinel /usr/local/redis/etc/sentinel-27000.conf
    redis-sentinel /usr/local/redis/etc/sentinel-27001.conf
    redis-sentinel /usr/local/redis/etc/sentinel-27002.conf

然后手动关闭主节点的redis服务，并查看两个slave信息是否有一个变成了master。

#### 配置以脚本方式启动

在redis的bin目录下新建redis-replication-sentinel-startup.sh和redis-replication-sentinel-shutdown.sh
startup.sh内容如下:

    redis-server /usr/local/redis/etc/redis-master-7000.conf
    redis-server /usr/local/redis/etc/redis-slave-7001.conf
    redis-server /usr/local/redis/etc/redis-slave-7002.conf

    redis-sentinel /usr/local/redis/etc/sentinel-27000.conf
    redis-sentinel /usr/local/redis/etc/sentinel-27001.conf
    redis-sentinel /usr/local/redis/etc/sentinel-27002.conf
    echo started。。。。。。。

shutdown.sh内容如下:
    
    redis-cli -p 7000 shutdown
    redis-cli -p 7001 shutdown
    redis-cli -p 7002 shutdown

    redis-cli -p 27000 shutdown
    redis-cli -p 27001 shutdown
    redis-cli -p 27002 shutdown

为两个文件添加可执行权限:

    sudo chmod +x <filename>


参考链接:[Redis高可用方案](https://www.jianshu.com/p/7d5fbf90bcd7)
未完待续。。。
