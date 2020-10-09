---
title: VMWare Centos7 NAT模式
date: 2020-08-10 15:32:00
tags:
- Centos7
- Linux
---

## 1、VMware设置

点击 编辑 -> 虚拟网络编辑器 -> 更改设置 

![img](D:\Photos\1.jpg)

选择 NAT模式，具体勾选如下：

![img](D:\Photos\2.jpg)

打开 NAT设置，记录子网掩码，网关ip

![img](D:\Photos\3.jpg)

记录网段信息

![img](D:\Photos\ccd0b119b11e4b9bb02d3202286a76ab.jpg)

## 2、登录虚拟机

root用户登录虚拟机，输入以命令，编辑保存







```
vi /etc/sysconfig/network-scripts/ifcfg-ens33

ONBOOT=yes
IPADDR=192.168.186.130
METMASK=255.255.255.0
GATEWAY=192.168.186.2
DNS=8.8.8.8
```



![1](D:\Photos\5.jpg)

输入一下命令，重启network

```
systemctl restart network
```



## 3、验证

  输入以下命令，查看结果



```
ping www.baidu.com
```
