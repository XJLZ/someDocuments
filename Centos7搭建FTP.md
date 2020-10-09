---
title: Centos7搭建FTP
date: 2020-08-10 15:32:00
tags:
- ftp
- Centos7
---

#### 安装前准备

vsftpd是linux下的一款小巧轻快，安全易用的FTP服务器软件，是一款在各个Linux发行版中最受推崇的FTP服务器软件。

1.安装vsftpd，直接yum 安装就可以了

```
yum install -y vsftpd
```

2.相关配置文件：

```
cd /etc/vsftpd/
```

3.启动服务

`systemctl enable vsftpd.service` //设置开机自启动

`systemctl start vsftpd.service` //启动ftp服务

`netstat -antup | grep ftp` //查看ftp服务端口

3.修改相关配置文件

```
vim /etc/vsftpd/vsftpd.conf
```

#### 创建ftp用户

1.创建用户并指定目录

```
useradd -d /var/www/ftpData admin  #目录自己创建  
passwd admin  #(回车)给用户admin设置登录密码
```

2.修改配置文件

```
userlist_enable=YES     #启动用户列表
userlist_deny=NO        #决定是否对用户列表的用户拒绝访问ftp 
userlist_file=/etc/vsftpd/user_list
```

3.在user_list中写入 ftp这个用户

4.配置vsftpd.conf 锁定根目录

```
local_root= /var/www        #本地用户登录后自动转到的ftp根目录
chroot_local_user=YES       #将所有用户限定在指定的主目录内
chroot_list_enable=NO       #不启用列外的用户列表
chroot_list_file=/etc/vsftpd/chroot_list  #指定列外的用户列表文件 此文件是让用户登录后可以查看其他目录，若要使用户仅在指定的ftp目录，该文件就不要填写该用户
```

5.修改ftp目录的权限,将用户添加到root组

```
usermod -g root admin
chown admin:root /var/www/ftpData
！！添加到root用户组后需要修改配置文件，加上以下配置：
allow_writeable_chroot=YES
```

