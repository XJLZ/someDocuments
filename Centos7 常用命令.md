---
title: Centos7 常用命令
date: 2020-08-10 15:32:00
tags:
- Centos7
---

#### 开端口

```
【单个】
firewall-cmd --zone=public --add-port=2181/tcp --permanent
【范围】
firewall-cmd --zone=public --add-port=30000-30999/tcp --permanent
【删除】
firewall-cmd --zone=public --remove-port=8080/tcp --permanent
【刷新配置】
firewall-cmd --reload
#找出公共区域的所有设置
firewall-cmd --zone=public --list-all
firewall-cmd --list-all
```

####  重启防火墙：

```
systemctl restart firewalld.service
systemctl stop firewalld.service
```

####  开机启动配置

```
/etc/rc.local
可加入命令，如：nohup java -jar text.jar
```

#### shell脚本执行报错（ /bin/bash^M: 坏的解释器: 没有那个文件或目录）

```
sed -i "s/\r//" install.sh
```

#### 查看系统版本

```
cat /proc/version
cat /etc/redhat-release
```

#### 查看文件/文件夹大小

```
指定目录的总大小，可以使用 du -sh 目录名称，du -sh test/ 或 du -h test/
当前目录大小 du -sh 或  du -h
文件大小 du -h index.html
```

#### 查看系统时间

- 查看系统时间的命令： date

#### 查看硬件时间

- 查看硬件时间的命令：  hwclock

#### 时间服务器上的时间同步的方法

```
安装ntpdate工具
# yum -y install ntp ntpdate

设置系统时间与网络时间同步
# ntpdate cn.pool.ntp.org

将系统时间写入硬件时间
# hwclock --systohc
```

#### 安装net-tools

```
yum install -y net-tools.x86_64 
```

#### 安装lrzsz

```
yum -y install lrzsz
```

w1sTqlD8QvFsx/aoSPrmfZMQkAwAxy1y+eEXwEc7GiYb4Q5gujLos4M0xfu77S2ZR8rr0hwMoqKDLFVHA5PxayGHy8lMyZSn5K3vCcjR/4Z8vEyzGReK+EgucnatTkfgOOys1wS02UcoQfX5Cffg0PIhhPokggSTGk5gxBizoW9kvAmzspCJEQ+v3Gq9Q5bZlugpdiwJ3bbLiW/OHg1iZq39mAxXcvaPbWOJmtqhbmkDm/VCZ/zFQ1vf8Xi32rseazqFJsPhLOcZLdqLfwOjqoSJuc9G9mCQIxbtskBD9wQOP3AVmdhtEg6d+en4uhypJdf7UEUAWjCj2JO5BOg75ZAbnEr72s89N9HWsydsPQSWSJbmF75X2E22qg7naKjZZBSJZez45i7j5FgHp1pt82zvIWIlAItpYeH00Kkn96eUQ0ODQJ0wodg2Pfoi4Vu46wG3jC/1J0oOWBP2uzCy3JA+rqCgYAtB+NNhAidtwYPk4Nu1HxNFHKDRNo5MMKr/1ujMIBR2+4Eft2S1Mq7eq3vOrz5+AQdxv+rjRxyKMFaBHXKYxYU/6YfrqLqxqjx31JLVRMPdS5s5z58GosjqHNFPQY0Bhokc6ZauqSFelY/M2AHUdnlp4/YATZhjteIyJLIprT0UyYEiidwwtxzlCNfqyKLhfC72F0bB+n8TKIs=