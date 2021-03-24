---
title: Centos7安装elasticsearch
date: 2020-11-19 09:32:00
tags:
- elasticsearch
- Centos7
---

#### 下载安装包

官网地址  https://www.elastic.co/cn/downloads/past-releases#elasticsearch

选择自己要下载的版本，这里以v7.3.2为例

#### 安装&脚本

​	es是要用非root用户启动

```
#!/bin/bash
data="/usr/local"
# 设置用户和用户组
user="es"
group="elasticsearch"
password="123456"
groupadd ${group}

useradd ${user}

passwd ${user} ${password}

usermod -G ${group} ${user}

tar -xvf elasticsearch-7.3.2-linux-x86_64.tar.gz

sudo mv elasticsearch-7.3.2 ${data}/es

sudo chown -R ${user}:${group} ${data}/es

echo "vm.max_map_count=262144" >> /etc/sysctl.conf

sysctl -p 
# 修改每个进程最大同时打开文件数和最大线程个数
echo "* hard nofile 65536" >> /etc/security/limits.conf
echo "* soft nofile 65536" >> /etc/security/limits.conf
echo "* soft nproc 4096" >> /etc/security/limits.conf
echo "* hard nproc 4096" >> /etc/security/limits.conf

```

#### 安装插件

如：ik分词插件

下载安装包 https://github.com/medcl/elasticsearch-analysis-ik/releases

解压到刚刚安装的es目录 如： /usr/local/es/plugins

#### 配置elasticsearch.yml（无密码）

进入安装目录的config目录，编辑vim elasticsearch.yml

单节点配置：

主要修改：

node.name: node-1

network.host: 192.168.186.140

http.port: 9200

cluster.initial_master_nodes: ["node-1"]

```
# ======================== Elasticsearch Configuration =========================
#
# NOTE: Elasticsearch comes with reasonable defaults for most settings.
#       Before you set out to tweak and tune the configuration, make sure you
#       understand what are you trying to accomplish and the consequences.
#
# The primary way of configuring a node is via this file. This template lists
# the most important settings you may want to configure for a production cluster.
#
# Please consult the documentation for further information on configuration options:
# https://www.elastic.co/guide/en/elasticsearch/reference/index.html
#
# ---------------------------------- Cluster -----------------------------------
#
# Use a descriptive name for your cluster:
#
#cluster.name: my-application
#
# ------------------------------------ Node ------------------------------------
#
# Use a descriptive name for the node:
#
node.name: node-1
#
# Add custom attributes to the node:
#
#node.attr.rack: r1
#
# ----------------------------------- Paths ------------------------------------
#
# Path to directory where to store the data (separate multiple locations by comma):
#
#path.data: /path/to/data
#
# Path to log files:
#
#path.logs: /path/to/logs
#
# ----------------------------------- Memory -----------------------------------
#
# Lock the memory on startup:
#
#bootstrap.memory_lock: true
#
# Make sure that the heap size is set to about half the memory available
# on the system and that the owner of the process is allowed to use this
# limit.
#
# Elasticsearch performs poorly when the system is swapping the memory.
#
# ---------------------------------- Network -----------------------------------
#
# Set the bind address to a specific IP (IPv4 or IPv6):
#
network.host: 192.168.186.140
#
# Set a custom port for HTTP:
#
http.port: 9200
#
# For more information, consult the network module documentation.
#
# --------------------------------- Discovery ----------------------------------
#
# Pass an initial list of hosts to perform discovery when this node is started:
# The default list of hosts is ["127.0.0.1", "[::1]"]
#
#discovery.seed_hosts: ["host1", "host2"]
#
# Bootstrap the cluster using an initial set of master-eligible nodes:
#
cluster.initial_master_nodes: ["node-1"]
#
# For more information, consult the discovery and cluster formation module documentation.
#
# ---------------------------------- Gateway -----------------------------------
#
# Block initial recovery after a full cluster restart until N nodes are started:
#
#gateway.recover_after_nodes: 3
#
# For more information, consult the gateway module documentation.
#
# ---------------------------------- Various -----------------------------------
#
# Require explicit names when deleting indices:
#
#action.destructive_requires_name: true
#
#
#http.cors.enabled: true
#http.cors.allow-origin: "*"
#http.cors.allow-headers: "X-Requested-With, Content-Type, Content-Length, X-User"
#xpack.security.enabled: true

#xpack.security.transport.ssl.enabled: true

```

#### 配置elasticsearch.yml（有密码）

编辑vim elasticsearch.yml

添加

```
http.cors.enabled: true
http.cors.allow-origin: "*"
http.cors.allow-headers: "X-Requested-With, Content-Type, Content-Length, X-User"
xpack.security.enabled: true

xpack.security.transport.ssl.enabled: true
```

非root用户重启elasticsearch服务

非root用户执行./elasticsearch-setup-passwords interactive   自定义密码

![image-20201119102153473](https://i.loli.net/2020/11/19/kONVlUxq5BFfyvC.png)

#### 修改kibana配置文件（有密码）

**1.修改kibana.yml配置文件，添加以下配置**

```
#进入kibana安装目录
cd /usr/local/kibana-7.2.0-linux-x86_64/config

#修改配置文件
vim kibana.yml

#添加配置
elasticsearch.username: "elastic"
elasticsearch.password: "xxx"
```

**2.非root用户重启kibana服务**

**3.登录kibana**



#### 常用命令

##### 1.查询索引文档数量

```
curl --noproxy '*' --user 'user:passwd' -X GET 'http://ip:9200/_cat/count/index_name?v'

epoch      timestamp count
1614838001 06:06:41  122090


curl --noproxy '*' --user 'user:passwd' -X GET 'http://ip:9200/_cat/indices?v
```

##### 2.查看es状态

```
curl --noproxy '*' --user 'user:passwd' -X GET 'http://ip:9200/_cluster/health?pretty'

{
  "cluster_name" : "fx",
  "status" : "yellow",
  "timed_out" : false,
  "number_of_nodes" : 1,
  "number_of_data_nodes" : 1,
  "active_primary_shards" : 22,
  "active_shards" : 22,
  "relocating_shards" : 0,
  "initializing_shards" : 0,
  "unassigned_shards" : 11,
  "delayed_unassigned_shards" : 0,
  "number_of_pending_tasks" : 0,
  "number_of_in_flight_fetch" : 0,
  "task_max_waiting_in_queue_millis" : 0,
  "active_shards_percent_as_number" : 66.66666666666666
}
```

##### 3.删除索引下的数据

```
curl --noproxy '*' --user 'user:passwd' -X POST 'http:ip//:9200/cpsl_library_v1.12/_delete_by_query?pretty' -d '{"query": {"match_all": {}}}'

curl --noproxy '*' --user 'elastic:v2m$yYlvN!' -X POST  'http://ip:9200/cpsl_library_v1.12/_delete_by_query?refresh&pretty' -H 'Content-Type: application/json' -d '{"query": {"match_all": {}}}'
```

