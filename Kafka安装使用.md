---
title: Kafka安装使用
date: 2020-10-29 11:32:00
tags:
- Centos7
- Kafka
- Spring-Boot
---

#### 下载安装

1.[官网下载](https://kafka.apachecn.org/downloads.html)

2.解压 tar -zxvf  xxxxxxx.tar.gz

#### 配置

```

############################# Server Basics #############################

# broker的id，要求唯一，集群部署不能重复
broker.id=0
############################# Socket Server Settings #############################

# 监听的服务端口，所部署机器的IP或者域名＋端口，记得防火墙开端口 
# 监听器列表 - 使用逗号分隔URI列表和监听器名称。如果侦听器名称不是安全协议，则还必须设置listener.security.protocol.map。指定主机名为0.0.0.0来绑定到所有接口。留空则绑定到默认接口上
# java.net.InetAddress.getCanonicalHostName() if not configured.
#   FORMAT:
#     listeners = listener_name://host_name:port
#   EXAMPLE:
#     listeners = PLAINTEXT://your.host.name:9092
listeners=PLAINTEXT://192.168.186.144:9092

# Hostname and port the broker will advertise to producers and consumers. If not set, 
# it uses the value for "listeners" if configured.  Otherwise, it will use the value
# returned from java.net.InetAddress.getCanonicalHostName().
#advertised.listeners=PLAINTEXT://your.host.name:9092

# Maps listener names to security protocols, the default is for them to be the same. See the config documentation for more details
#listener.security.protocol.map=PLAINTEXT:PLAINTEXT,SSL:SSL,SASL_PLAINTEXT:SASL_PLAINTEXT,SASL_SSL:SASL_SSL

# 服务器用于从接收网络请求并发送网络响应的线程数
num.network.threads=3

# 服务器用于处理请求的线程数，可能包括磁盘I/O
num.io.threads=8

# 服务端用来处理socket连接的SO_SNDBUF缓冲大小。如果值为-1，则使用系统默认值。
socket.send.buffer.bytes=102400

# 服务端用来处理socket连接的SO_RCVBUFF缓冲大小。如果值为-1，则使用系统默认值
socket.receive.buffer.bytes=102400

# socket请求的最大大小，这是为了防止server跑光内存，不能大于Java堆的大小。（104857600->100MB）
socket.request.max.bytes=104857600


############################# Log Basics #############################

# 保存日志数据的目录，如果未设置将使用log.dir的配置。
log.dirs=/home/cloud/server/kafka/kafka/logs

# ！！每个topic的默认日志分区数 ！！
num.partitions=3

# 每个数据目录，用于启动时日志恢复和关闭时刷新的线程数
num.recovery.threads.per.data.dir=1

############################# Internal Topic Settings  #############################
# The replication factor for the group metadata internal topics "__consumer_offsets" and "__transaction_state"
# For anything other than development testing, a value greater than 1 is recommended for to ensure availability such as 3.

# offset topic的副本数（设置的越大，可用性越高）。内部topic创建将失败，直到集群大小满足此副本数要求。
offsets.topic.replication.factor=3
# 事务topic的副本数（设置的越大，可用性越高）。内部topic在集群数满足副本数之前，将会一直创建失败。
transaction.state.log.replication.factor=1
# 覆盖事务topic的min.insync.replicas配置
# 当producer将ack设置为“全部”（或“-1”）时，min.insync.replicas指定了被认为写入成功的最小副本数。
# 如果这个最小值不能满足，那么producer将会引发一个异常（NotEnoughReplicas或NotEnoughReplicasAfterAppend）。
# 当一起使用时，min.insync.replicas和acks允许您强制更大的耐久性保证。 
# 一个经典的情况是创建一个复本数为3的topic，将min.insync.replicas设置为2，并且producer使用“all”选项。 
# 这将确保如果大多数副本没有写入producer则抛出异常。
transaction.state.log.min.isr=1

############################# Log Flush Policy #############################

# Messages are immediately written to the filesystem but by default we only fsync() to sync
# the OS cache lazily. The following configurations control the flush of data to disk.
# There are a few important trade-offs here:
#    1. Durability: Unflushed data may be lost if you are not using replication.
#    2. Latency: Very large flush intervals may lead to latency spikes when the flush does occur as there will be a lot of data to flush.
#    3. Throughput: The flush is generally the most expensive operation, and a small flush interval may lead to exceessive seeks.
# The settings below allow one to configure the flush policy to flush data after a period of time or
# every N messages (or both). This can be done globally and overridden on a per-topic basis.

# 在将消息刷新到磁盘之前，在日志分区上累积的消息数量。默认：	9223372036854775807
#log.flush.interval.messages=10000

# 在刷新到磁盘之前，任何topic中的消息保留在内存中的最长时间（以毫秒为单位）。如果未设置，则使用
# 如果未设置，则使用log.flush.scheduler.interval.ms中的值。 
#log.flush.interval.ms=1000

############################# Log Retention Policy #############################

# The following configurations control the disposal of log segments. The policy can
# be set to delete segments after a period of time, or after a given size has accumulated.
# A segment will be deleted whenever *either* of these criteria are met. Deletion always happens
# from the end of the log.

# The minimum age of a log file to be eligible for deletion due to age
log.retention.hours=168

# A size-based retention policy for logs. Segments are pruned from the log unless the remaining
# segments drop below log.retention.bytes. Functions independently of log.retention.hours.
#log.retention.bytes=1073741824

# The maximum size of a log segment file. When this size is reached a new log segment will be created.
log.segment.bytes=1073741824

# The interval at which log segments are checked to see if they can be deleted according
# to the retention policies
log.retention.check.interval.ms=300000

############################# Zookeeper #############################

# Zookeeper connection string (see zookeeper docs for details).
# This is a comma separated host:port pairs, each corresponding to a zk
# server. e.g. "127.0.0.1:3000,127.0.0.1:3001,127.0.0.1:3002".
# You can also append an optional chroot string to the urls to specify the
# root directory for all kafka znodes.
zookeeper.connect=localhost:2181

# Timeout in ms for connecting to zookeeper
zookeeper.connection.timeout.ms=6000


############################# Group Coordinator Settings #############################

# The following configuration specifies the time, in milliseconds, that the GroupCoordinator will delay the initial consumer rebalance.
# The rebalance will be further delayed by the value of group.initial.rebalance.delay.ms as new members join the group, up to a maximum of max.poll.interval.ms.
# The default value for this is 3 seconds.
# We override this to 0 here as it makes for a better out-of-the-box experience for development and testing.
# However, in production environments the default value of 3 seconds is more suitable as this will help to avoid unnecessary, and potentially expensive, rebalances during application startup.
group.initial.rebalance.delay.ms=0

```

#### 启动zk

```
./bin/zkServer.sh start
```

#### 启动kafka

```
nohup ./bin/kafka-server-start.sh config/server.properties > server.out &
nohup ./bin/kafka-server-start.sh config/server-1.properties > server1.out &
nohup ./bin/kafka-server-start.sh config/server-2.properties > server2.out &
```

#### 创建topic

```
bin/kafka-topics.sh --create --zookeeper localhost:2181 --replication-factor 1 --partitions 1 --topic kafka-stream1
```



#### 查看topic

```
# 查看topic列表
./bin/kafka-topics.sh --list --zookeeper 192.168.186.140:2181
# 查看topic描述
./bin/kafka-topics.sh --describe --zookeeper 192.168.186.140:2181 --topic kafka-test
# 删除topic
./bin/kafka-topics.sh --delete --zookeeper 192.168.186.140:2181 --topic topic1
```

#### 查看消费者组

```
./bin/kafka-consumer-groups.sh --new-consumer --bootstrap-server 192.168.186.140:9092 --list

# 查看特定组信息
./bin/kafka-consumer-groups.sh --bootstrap-server 192.168.186.140:9092 --group kafka_tran_g1 --describe
```

#### 消费者

```
./bin/kafka-console-consumer.sh --bootstrap-server 192.168.186.140:9092 --from-beginning --topic kafka-test

./bin/kafka-console-consumer.sh --bootstrap-server 192.168.186.140:9092 --topic kafka-stream2
```

#### 生产者

```
./bin/kafka-console-producer.sh --broker-list 192.168.186.140:9092 --topic kafka-stream1
```

