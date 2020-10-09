---
title: MySQ5.7脚本安装
date: 2020-08-10 15:32:00
tags:
- Centos7
- MySQL
---

#### 脚本

```
#!/bin/bash
#压缩包目录
data="home/mysql"

echo "将mysql压缩包解压"

tar xvf mysql-5.7.30-linux-glibc2.12-x86_64.tar.gz 

echo "新建mysql文件夹"
if [ ! -d "/"${data}"/mysql-5.7.30-linux-glibc2.12-x86_64" ]; then
        echo "mysql解压未完成"
        exit
fi

echo "mysql解压完成修改文件夹"

mv mysql-5.7.30-linux-glibc2.12-x86_64 mysql

mv mysql /usr/local

echo "mysql解压完成"

echo "切换目录"

cd /usr/local/mysql/

echo "新建数据库data目录"

mkdir data

echo "初始化mysql"

cd /usr/local/mysql/

bin/mysqld --initialize --user=root --basedir=/usr/local/mysql --datadir=/usr/local/mysql/data


echo "修改配置文件"
cat /dev/null > /etc/my.cnf
if [ -f "/etc/my.cnf" ]; then
        mv /etc/my.cnf /etc/my.cnf.bak
fi

echo '[mysqld]' >> /etc/my.cnf
echo 'bind-address=0.0.0.0' >> /etc/my.cnf
echo 'basedir = /usr/local/mysql' >> /etc/my.cnf
echo 'datadir = /usr/local/mysql/data' >> /etc/my.cnf
echo 'socket=/tmp/mysql.sock' >> /etc/my.cnf
echo 'user=root' >> /etc/my.cnf
echo 'port=3306' >> /etc/my.cnf
echo 'character-set-server=utf8' >> /etc/my.cnf
#echo 'skip-grant-tables' >> /etc/my.cnf
echo '# Disabling symbolic-links is recommended to prevent assorted security risks' >> /etc/my.cnf
echo 'symbolic-links=0' >> /etc/my.cnf
echo '# skip-grant-tables' >> /etc/my.cnf
echo '[mysqld_safe]' >> /etc/my.cnf
echo 'log-error=/var/log/mysqld.log' >> /etc/my.cnf
echo 'pid-file=/var/run/mysqld/mysqld.pid' >> /etc/my.cnf
echo '!includedir /etc/my.cnf.d' >> /etc/my.cnf
echo '[client]' >> /etc/my.cnf
echo 'user=root' >> /etc/my.cnf
echo 'password=123456' >> /etc/my.cnf
echo 'port=3306' >> /etc/my.cnf



echo "配置修改完成，将mysql加入服务"

cp /usr/local/mysql/support-files/mysql.server /etc/init.d/mysqld

#chmod +x /etc/init.d/mysqld


echo "添加mysql指令"

ln -s /usr/local/mysql/bin/mysql  /usr/local/bin
ln -s /usr/local/mysql/bin/mysqladmin  /usr/local/bin

#echo "服务启动"

sudo service mysqld start

sudo systemctl restart mysqld

systemctl status mysqld


```

1、 修改用户密码

```
mysql> alter user 'root'@'localhost' identified by 'root';  
刷新权限
mysql> flush privileges;
```

2、远程登录

```
mysql> update user set host = '%' where user = 'root';
mysql> grant all privileges on *.* to  root@'%' identified by 'root';
刷新权限
mysql> flush privileges;
```

3、查找并删除mysql有关的文件

```swift
find / -name mysql
rm -rf 上边查找到的路径，多个路径用空格隔开
#或者下边一条命令即可
find / -name mysql|xargs rm -rf
```

