---
title: Centos7 常用命令
date: 2020-08-10 15:32:00
tags:
- Centos7
---

#### 开端口

```
【单个】
firewall-cmd --zone=public --add-port=20/tcp --permanent
【范围】
firewall-cmd --zone=public --remove-port=8000-8900/tcp --permanent
【删除】
firewall-cmd --zone=public --remove-port=80/tcp --permanent  
#找出公共区域的所有设置
firewall-cmd --zone=public --list-all
firewall-cmd --list-all
```

####  重启防火墙：

```
systemctl restart firewalld.service
systemctl stop firewalld.service
```

 
