---
title: httpd
date: 2020-08-10 15:32:00
tags:
- Centos7
- httpd
---



#### 安装

```
yum -y install httpd.x86_64
```





#### 配置文件(位置：/etc/httpd/conf/httpd.conf)

```
Listen 80
Listen 192.168.31.30:8080

<virtualhost 192.168.31.30:8080>
        Documentroot /home/diskdata/ftp/minioData
        <Directory "/home/diskdata/ftp/minioData">
            Options FollowSymLinks
            AllowOverride none
            Require all granted
        </Directory>
</virtualhost>
```



#### 第一、启动、终止、重启

```
systemctl start httpd.service #启动
systemctl stop httpd.service #停止
systemctl restart httpd.service #重启
```



#### 第二、设置开机启动/关闭

```
systemctl enable httpd.service #开机启动
systemctl disable httpd.service #开机不启动
```



#### 第三、检查httpd状态

```
systemctl status httpd.service
```





#### 解决启动Apache遇到的问题Permission denied: AH00072: make_sock: could not bind to address 0.0.0.0:8888

[SELunix]: SELinux.md

```
1.可能原因：SELinux限制了Apache的端口设置
2.关闭SELinux
setenforce 0 ##设置SELinux 成为permissive模式 √
3.
//安装semanage
yum provides /usr/sbin/semanage
yum -y install policycoreutils-python
//查看默认允许的端口
semanage port -l | grep -w http_port_t
// http_port_t  tcp 80, 81, 443, 488, 8008, 8009, 8443, 9000
//使用semanage添加apache侦听的端口
semanage port -a -t http_port_t -p tcp 8888
//启动apache
systemctl start httpd
```

