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

#跳过密码验证
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

#开启端口
firewall-cmd --zone=public --add-port=3306/tcp --permanent

systemctl restart firewalld.service

#修改root初始化密码
mysql -uroot -p < /home/mirror/mysql/init.sql

#关闭跳过密码验证
cat /dev/null > /etc/my.cnf
echo '[mysqld]' >> /etc/my.cnf
echo 'bind-address=0.0.0.0' >> /etc/my.cnf
echo 'basedir=/usr/local/mysql' >> /etc/my.cnf
echo 'datadir=/usr/local/mysql/data' >> /etc/my.cnf
echo 'socket=/tmp/mysql.sock' >> /etc/my.cnf
echo 'user=root' >> /etc/my.cnf
echo 'port=3306' >> /etc/my.cnf
echo 'character-set-server=utf8' >> /etc/my.cnf
echo '#skip-grant-tables' >> /etc/my.cnf
echo '# Disabling symbolic-links is recommended to prevent assorted security risks' >> /etc/my.cnf
echo 'symbolic-links=0' >> /etc/my.cnf
echo '[mysqld_safe]' >> /etc/my.cnf
echo 'log-error=/var/log/mysqld.log' >> /etc/my.cnf
echo 'pid-file=/var/run/mysqld/mysqld.pid' >> /etc/my.cnf
echo '!includedir /etc/my.cnf.d' >> /etc/my.cnf

sudo systemctl restart mysqld

sudo systemctl status mysqld
```

#### 1、 修改用户密码（init.sql）

```
flush privileges;
#修改root密码为root
alter user 'root'@'localhost' identified by 'root';  
#刷新权限
flush privileges;
```

#### 2、远程登录

```
use mysql;
update user set host = '%' where user = 'root';
#刷新权限
flush privileges;
```

#### 3、添加用户

```
#添加用户“xiao"，密码为: aquatic@zkcm
create user 'cpbdb'@'%' identified by 'cpbdb@zkcm';
#赋权所有权限，并且可以查看所有表
grant all privileges on *.* to 'cpbdb'@'%';
#赋权test数据库所有权限，只可以使用数据库test
grant all privileges on cpbdb.* to 'cpbdb'@'%';
#刷新权限
flush privileges;

revoke all privileges on *.* from 'cpbdb'@'%';
```



#### 4、查找并删除mysql有关的文件

```swift
find / -name mysql
rm -rf 上边查找到的路径，多个路径用空格隔开
#或者下边一条命令即可
find / -name mysql|xargs rm -rf
```

#### 5、建库

```
CREATE DATABASE `fx` DEFAULT CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci;
CREATE DATABASE `cpbdb-backup` DEFAULT CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci;
CREATE DATABASE `cpbdb-dev` DEFAULT CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci;
CREATE DATABASE `cpbdb-dev-test` DEFAULT CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci;
CREATE DATABASE `cpbdb` DEFAULT CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci;
```

#### 6、导出

```
-- 只导出数据
./mysqldump -t cpbdb -uroot -proot > /home/cpbdb.sql

-- 只导出表结构
./mysqldump -d cpbdb -uroot -proot > /home/cpbdb.sql

-- 导出整个数据库
./mysqldump -uroot -proot cpbdb > /home/cpbdb.sql
```

#### 7、查询表结构

```SQL
SELECT
  TABLE_NAME 表名,
  COLUMN_NAME 列名,
  COLUMN_TYPE 数据类型,
	COLUMN_DEFAULT 默认值,
  IS_NULLABLE 强制,	
  COLUMN_COMMENT 注释 
FROM
 INFORMATION_SCHEMA.COLUMNS
where
-- db2为数据库名称，到时候只需要修改成你要导出表结构的数据库即可
table_schema ='db2'
and table_name = 't_author_area'
```

![image-20210330163131838](https://i.loli.net/2021/03/30/qHUOgFj9o3WCpnR.png)

| 列名                         | 数据类型           | 描述                                                         |
| :--------------------------- | :----------------- | :----------------------------------------------------------- |
| **TABLE_CATALOG**            | **nvarchar(128)**  | 表限定符。                                                   |
| **TABLE_SCHEMA**             | **nvarchar(128)**  | 表所有者。                                                   |
| **TABLE_NAME**               | **nvarchar(128)**  | 表名。                                                       |
| **COLUMN_NAME**              | **nvarchar(128)**  | 列名。                                                       |
| **ORDINAL_POSITION**         | **smallint**       | 列标识号。                                                   |
| **COLUMN_DEFAULT**           | **nvarchar(4000)** | 列的默认值。                                                 |
| **IS_NULLABLE**              | **varchar(3)**     | 列的为空性。如果列允许 NULL，那么该列返回 YES。否则，返回 NO。 |
| **DATA_TYPE**                | **nvarchar(128)**  | 系统提供的数据类型。                                         |
| **CHARACTER_MAXIMUM_LENGTH** | **smallint**       | 以字符为单位的最大长度，适于二进制数据、字符数据，或者文本和图像数据。否则，返回 NULL。有关更多信息，请参见数据类型。 |
| **CHARACTER_OCTET_LENGTH**   | **smallint**       | 以字节为单位的最大长度，适于二进制数据、字符数据，或者文本和图像数据。否则，返回 NULL。 |
| **NUMERIC_PRECISION**        | **tinyint**        | 近似数字数据、精确数字数据、整型数据或货币数据的精度。否则，返回 NULL。 |
| **NUMERIC_PRECISION_RADIX**  | **smallint**       | 近似数字数据、精确数字数据、整型数据或货币数据的精度基数。否则，返回 NULL。 |
| **NUMERIC_SCALE**            | **tinyint**        | 近似数字数据、精确数字数据、整数数据或货币数据的小数位数。否则，返回 NULL。 |
| **DATETIME_PRECISION**       | **smallint**       | **datetime** 及 SQL-92 **interval** 数据类型的子类型代码。对于其它数据类型，返回 NULL。 |
| **CHARACTER_SET_CATALOG**    | **varchar(6)**     | 如果列是字符数据或 **text** 数据类型，那么返回 **master**，指明字符集所在的数据库。否则，返回 NULL。 |
| **CHARACTER_SET_SCHEMA**     | **varchar(3)**     | 如果列是字符数据或 **text** 数据类型，那么返回 **DBO**，指明字符集的所有者名称。否则，返回 NULL。 |
| **CHARACTER_SET_NAME**       | **nvarchar(128)**  | 如果该列是字符数据或 **text** 数据类型，那么为字符集返回唯一的名称。否则，返回 NULL。 |
| **COLLATION_CATALOG**        | **varchar(6)**     | 如果列是字符数据或 **text** 数据类型，那么返回 **master**，指明在其中定义排序次序的数据库。否则此列为 NULL。 |
| **COLLATION_SCHEMA**         | **varchar(3)**     | 返回 **DBO**，为字符数据或 **text** 数据类型指明排序次序的所有者。否则，返回 NULL。 |
| **COLLATION_NAME**           | **nvarchar(128)**  | 如果列是字符数据或 **text** 数据类型，那么为排序次序返回唯一的名称。否则，返回 NULL。 |
| **DOMAIN_CATALOG**           | **nvarchar(128)**  | 如果列是一种用户定义数据类型，那么该列是某个数据库名称，在该数据库名中创建了这种用户定义数据类型。否则，返回 NULL。 |
| **DOMAIN_SCHEMA**            | **nvarchar(128)**  | 如果列是一种用户定义数据类型，那么该列是这种用户定义数据类型的创建者。否则，返回 NULL。 |
| **DOMAIN_NAME**              | **nvarchar(128)**  | 如果列是一种用户定义数据类型，那么该列是这种用户定义数据类型的名称。否则，返回 NULL。 |