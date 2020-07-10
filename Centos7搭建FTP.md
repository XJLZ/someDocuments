#### 安装前准备

vsftpd是linux下的一款小巧轻快，安全易用的FTP服务器软件，是一款在各个Linux发行版中最受推崇的FTP服务器软件。

1.安装vsftpd，直接yum 安装就可以了

```
yum install -y vsftpd
```

2.相关配置文件：

```
cd /etc/vsftpd
```

3.启动服务

`systemctl enable vsftpd.service` //设置开机自启动

`systemctl start vsftpd.service` //启动ftp服务

`netstat -antup | grep ftp` //查看ftp服务端口

4.另外简单介绍下vsftpd.conf的配置文件参数说明。

```
cat /etc/vsftpd/vsftpd.conf
```

[配置文件]: D:\ProgramFiles\Typora\Texts\vsftpd.conf

