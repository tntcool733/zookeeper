# zookeeper集群搭建

## 下载
+ 官方地址 - https://www.apache.org/dyn/closer.cgi/zookeeper/

## 解压并重命名
+ tar -zxvf zookeeper-3.4.14.tar.gz
+ cp三份zk_1, zk_2, zk_3

## 配置
+ 配置conf目录下的zoo.cfg
```
# The number of milliseconds of each tick
tickTime=2000
# The number of ticks that the initial
# synchronization phase can take
initLimit=10
# The number of ticks that can pass between
# sending a request and getting an acknowledgement
syncLimit=5
# the directory where the snapshot is stored.
# do not use /tmp for storage, /tmp here is just
# example sakes.
dataDir=/Users/tntcool733/zookeeper/zk_1/zookeeper-3.4.14/data
# the port at which the clients will connect
clientPort=6181
# the maximum number of client connections.
# increase this if you need to handle more clients
#maxClientCnxns=60
#
# Be sure to read the maintenance section of the
# administrator guide before turning on autopurge.
#
# http://zookeeper.apache.org/doc/current/zookeeperAdmin.html#sc_maintenance
#
# The number of snapshots to retain in dataDir
#autopurge.snapRetainCount=3
# Purge task interval in hours
# Set to "0" to disable auto purge feature
#autopurge.purgeInterval=1

# 如果是不参与选举的节点需要增加observer模式配置
#peerType=observer
#server.x=x.x.x.x:port1:prot2:observer

autopurge.snapRetainCount=10
autopurge.purgeInterval=48

# server.* - *是id标识
# ip - 主机ip
# 第一个port：leader通信所用prot
# 第二个port：当Leader宕机或其他故障时，集群进行重新选举Leader时使用的端口
server.1=127.0.0.1:6198:6199
server.2=127.0.0.1:6298:6299
server.3=127.0.0.1:6398:6399
```

+ 配置bin下面的zkEnv.sh，指定zk日志目录
```
ZOO_LOG_DIR="/Users/tntcool733/zookeeper/zk_1/zookeeper-3.4.14/"
```

+ 配置myid
```
1. 找到zoo.conf文件中data数据目录配置
2. 在data数据目录下创建myid文件，写入相应的id，如1，2，3等
```

## 启动
+ 启动
```
./zk_1/zookeeper-3.4.14/bin/zkServer.sh start
./zk_2/zookeeper-3.4.14/bin/zkServer.sh start
./zk_3/zookeeper-3.4.14/bin/zkServer.sh start
```

+ 观察状态
```
./zk_1/zookeeper-3.4.14/bin/zkServer.sh status
./zk_2/zookeeper-3.4.14/bin/zkServer.sh status
./zk_3/zookeeper-3.4.14/bin/zkServer.sh status
```

+ client连接
```
./zk_1/zookeeper-3.4.14/bin/zkCli.sh -server 127.0.0.1:6181,127.0.0.1:6182,127.0.0.1:6183
```

+ 观察日志
```
less /Users/tntcool733/zookeeper/zk_1/zookeeper-3.4.14/zookeeper.out
less /Users/tntcool733/zookeeper/zk_2/zookeeper-3.4.14/zookeeper.out
less /Users/tntcool733/zookeeper/zk_3/zookeeper-3.4.14/zookeeper.out
```

