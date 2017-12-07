---
title: redis-cluster
date: 2017-12-04 18:38:51
tags: redis
---

# redis-cluster 集群搭建

> 至少3台机器,3个master,3个slave,并且master和自己的slave不在同一台机器上,
>防止宕机时master无法恢复

## 重要的配置

+ cluster-enabled yes/no
+ cluster-config-file 让redis维护节点信息
+ cluster-node-timeout  `<milliseconds>` master宕机超过一定时间,主备切换

## 开始搭建

1.创建目录保存日志
+ `/etc/redis-cluster`
+ `/var/log/redis`

2.创建6个配置文件
```
port 7001
cluster-enabled yes
cluster-config-file /etc/redis-cluster/node-7001.conf

#5秒的超时时间
cluster-node-timeout 5000
daemonize yes
pidfile /var/run/redis_7001.pid
#数据保存的目录
dir     /var/redis/7001
logfile /var/log/redis/7001.log
# 节点的ip
bind xx.xx.xx.xx
appendonly yes

```

3.准备生产环境启动脚本,位于`/etc/init.d`

4.配置文件分布在3台机器的`/etc/redis`目录

5.启动6个redis实例

6.redis-tribe构建集群

7.添加master节点

+ 添加节点`redis-tribe.rb add-node <ip:port> <ip:port>`  
+ 检查cluster状态`redis-tribe.rb check <ip:port>`  
+ 迁移hashslot  `redis-tribe.rb reshard <ip:port>`

8.添加slave到master  
+ 创建启动脚本和conf文件
+ 启动redis
+ 挂载到master上`redis-tribe.rb add-node --slave --master-id <masterid> \  
<ip:port> <ip:port>`  
第一个是slave的ip

9.删除node  
reshard将数据移到其他节点,再用`redis-tribe del-node <ip:port> <node-id>`,`<ip:port>`  
是存储redis-cluster集群信息的节点  
redis会自动将hashslot为空的master的slave挂到其他master上
