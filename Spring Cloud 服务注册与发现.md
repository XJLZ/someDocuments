---
title: Spring Cloud 服务注册中心
date: 2020-12-18 09:32:00
tags:
- Spring Cloud
- 服务注册与发现
---

#### 什么是服务注册中心

​		服务注册中心就是，在微服务体系中，其中有一个服务是用来集中管理这些微服务实例，微服务间的调用只需要知道对方的服务名，而无需关注具体的IP和端口，便于微服务架构的拓展和维护。

#### CAP理论

​		CAP理论是分布式架构中重要理论，CAP不能都取，只能取其二

> - 一致性(Consistency) (所有节点在同一时间具有相同的数据)
> - 可用性(Availability) (保证每个请求不管成功或者失败都有响应)
> - 分区容错性(Partition tolerance) (系统中任意信息的丢失或失败不会影响系统的继续运作)

#### 常用的注册中心

> - Eureka
> - Zookeeper
> - Consul
> - Nacos

#### Eureka > AP

​		Eureka遵信AP原则，即高可用以及分隔容忍性，最终保证一致性，在Eureka Server集群中，由于其采用的是p2p对等通信，互相注册的方式，去除了中心化，所以只要还存在其中一个服务，服务就可以正常注册和发现，当宕机的Eureka Server可用时，就会请求复制操作，同步当前可用Eureka Server所有的服务节点

##### 		使用方式：

###### 		Eureka Server

​		在注册中心这个微服务中引入依赖

```xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-netflix-eureka-server</artifactId>
</dependency>
```

​		在application.yml中添加配置

```yml
eureka:
  instance:
    hostname: www.eureka7000.com #eureka服务端的应用实例主机名
  client:
    #false表示不向注册中心注册自己
    register-with-eureka: false
    #false表示自己端就是注册中心，我的职责就是维护服务实例，并不需要去检索服务
    fetch-registry: false
    service-url:
      #设置与Eureka Server交互的地址查询服务和注册服务都需要依赖这个地址
      #集群指向其它eureka
#      defaultZone: http://www.eureka7001.com:7001/eureka/
      #单机就是7000自己
      defaultZone: http://www.eureka7000.com:7000/eureka/
  server:
    # false 关闭自我保护机制，保证不可用服务呗及时剔除(不推荐)
#    enable-self-preservation: false
	# Eureka Server 清理无效节点的时间间隔（单位：毫秒）
#    eviction-interval-timer-in-ms: 2000
```

​		最后在该服务的启动类上加上注解：**@EnableEurekaServer**

集群方式启动：可以看到副本列表中有另一个Eureka Server的实例主机名

![image-20201218095906560](https://i.loli.net/2020/12/18/bzIGvP38Wwgd5SO.png)

![image-20201218095933475](https://i.loli.net/2020/12/18/ALwhEKFaOmxZNID.png)

###### 		**Eureka Client**

​		在资源微服务中引入依赖

```xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
</dependency>
```

​		在application.yml中添加配置

```yml
eureka:
  instance:
    # 隐藏域名
    instance-id: payment8001
    # 左下角访问路径可以显示IP地址
    prefer-ip-address: true
    # 向Eureka 服务端发送心跳的间隔时间，单位为秒，用于服务续约。这里配置为20秒，即每隔20秒向Eureka Server发送心跳，表明当前服务没有宕机；
#    lease-renewal-interval-in-seconds: 20
    # Eureka服务端在收到最后一次心跳后等待时间上限，（秒），默认90秒，超时剔除服务
#    lease-expiration-duration-in-seconds: 2
  client:
    #表示是否将自己注册进Eureka Server默认为true
    register-with-eureka: true
    #是否从Eureka Server抓取已有的注册信息，默认为true。单节点无所谓，集群必须设置为true才能配合ribbon使用负载均衡
    fetch-registry: true
    service-url:
    # 集群
      defaultZone: http://www.eureka7001.com:7001/eureka,http://www.eureka7000.com:7000/eureka
    # 单节点
#      defaultZone: http://www.eureka7000.com:7000/eureka
```

​	最后在该服务的启动类上加上注解：**@EnableEurekaClient**

​	可以看出，在使用了eureka.instance.instance-id之后隐藏了服务id以及端口

![image-20201218102313617](https://i.loli.net/2020/12/18/flqzcHDsYZEWNpX.png)

![image-20201218102421648](https://i.loli.net/2020/12/18/gYrUD3p5xMw4lHk.png)

​		当鼠标悬浮到“payment8002”上时，浏览器左下角显示IP地址

![image-20201218102707248](https://i.loli.net/2020/12/18/rAsZjkCJ3e4qMWm.png)



#### Zookeeper > CP

​	Zookeeper从设计模式角度来理解：是一个基于观察者模式设计（一个领导者(Leader)，多个跟随者(Follower)组成的集群）的分布式服务管理框架，它负责存储和管理大家都关心的数据，然后接受观察者的注册，一旦这些数据的状态发生变化，Zookeeper就将负责通知已经在Zookeeper上注册的那些观察者做出相应的反应。

​	Zookeeper遵循CP原则，保证容错和数据实时一致性，从 Zookeeper 的实际应用情况来看，在使用 Zookeeper 获取服务列表时，如果此时的 Zookeeper 集群中的 Leader 宕机了，该集群就要进行 Leader 的选举，又或者 Zookeeper 集群中半数以上服务器节点不可用（例如有三个节点，如果节点一检测到节点三挂了 ，节点二也检测到节点三挂了，那这个节点才算是真的挂了），那么将无法处理该请求。所以说，Zookeeper 不能保证服务可用性。

​		当然，在大多数分布式环境中，尤其是涉及到数据存储的场景，数据一致性应该是首先被保证的，这也是 Zookeeper 设计紧遵CP原则的另一个原因。

##### 		使用方式：

###### 		Server

​		首先Zookeeper注册中心需要单独下载

```
官网：https://zookeeper.apache.org/ 或 https://mirrors.tuna.tsinghua.edu.cn/apache/zookeeper/
```

​		解压并进入conf文件夹，修改配置文件，主要时修改dataDir位置对应的目录，**目录没有的先创建**

```
# 复制一份配置文件
cp zoo_sample.cfg zoo.cfg
# 编辑zoo.cfg
vim zoo.cfg
```

![image-20201218105618926](https://i.loli.net/2020/12/18/IiCh6VBKZxzmPtR.png)

​		修改好配置文件后，进入到bin目录

```
# 启动zk
./zkServer.sh start
# 开启端口2181
```

![image-20201218105800886](https://i.loli.net/2020/12/18/5JaA3siTq6bfzDW.png)

###### 		服务Client

​		在资源微服务中引入依赖

```xml
<dependency>
     <groupId>org.springframework.cloud</groupId>
     <artifactId>spring-cloud-starter-zookeeper-discovery</artifactId>
     <!--先排除自带的zookeeper3.5.3-->
     <exclusions>
         <exclusion>
             <groupId>org.apache.zookeeper</groupId>
             <artifactId>zookeeper</artifactId>
         </exclusion>
     </exclusions>
</dependency>
<!--添加zookeeper3.4.14版本-->
<dependency>
    <groupId>org.apache.zookeeper</groupId>
    <artifactId>zookeeper</artifactId>
    <version>3.4.14</version>
    <exclusions>
        <exclusion>
            <groupId>log4j</groupId>
            <artifactId>log4j</artifactId>
        </exclusion>
        <exclusion>
            <groupId>org.slf4j</groupId>
            <artifactId>slf4j-log4j12</artifactId>
        </exclusion>
    </exclusions>
</dependency>
```

​		在application.yml中添加配置

```yml
spring:
  application:
    name: cloud-consumer-order
  cloud:
    zookeeper:
      connect-string: 192.168.186.140:2181
      discovery:
        enabled: true
```

​		最后在启动类添加注解：**@EnableDiscoveryClient**

​		启动服务、执行命令./zkCli.sh进入zk客户端，执行 ls /services，查看注册进来的服务。

![image-20201218112909638](https://i.loli.net/2020/12/18/EBXknhrN5ZLGbo2.png)

#### Consul > CP

​		Consul遵循CP原则，保证了强一致性和分区容错性，且使用的是Raft算法，比zookeeper使用的Paxos算法更加简单。虽然保证了强一致性，但是可用性就相应下降了，例如服务注册的时间会稍长一些，因为 Consul 的 raft 协议要求必须过半数的节点都写入成功才认为注册成功 ；在leader挂掉了之后，重新选举出leader之前会导致Consul 服务不可用

##### 		使用方式：

###### 		Server

​		**Consul官网：**https://www.consul.io/intro/index.html，下载一个可执行文件

​		下载完成后解压

```
# 可视化界面启动，后台运行，并把日志输出到log.out
nohup ./consul agent -dev -client 0.0.0.0 -ui > log.out &
```

​		访问 IP:8500

![image-20201218132156223](https://i.loli.net/2020/12/18/jGC3fybYMao8QJ7.png)

###### 服务Client

​	在资源微服务中引入依赖

```xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-consul-discovery</artifactId>
</dependency>
```

​		在application.yml中添加配置

```yml
spring:
  application:
    name: cloud-consumer-order
  cloud:
    consul:
      host: 192.168.186.140
      port: 8500
      discovery:
        service-name: ${spring.application.name}
```

​		最后在启动类添加注解：**@EnableDiscoveryClient**

启动服务，刷新页面，可以看到服务已经注册进来了

![image-20201218132549369](https://i.loli.net/2020/12/18/EDCSLbK5kQ4wAhc.png)

点击进去可以看到该服务的状态以及信息

![image-20201218132723838](https://i.loli.net/2020/12/18/FKI8orfdjqUtLi3.png)

![image-20201218132747412](https://i.loli.net/2020/12/18/GP2tobIJS9NcM17.png)

