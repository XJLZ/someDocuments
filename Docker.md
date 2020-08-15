# 安装&配置

## 镜像加速

![1552711014072](C:\Users\Admin\AppData\Roaming\Typora\typora-user-images\1552711014072.png)

## 常用命令

### 基本命令

```
#获取镜像
docker  pull [镜像:xx.xx]
//docker pull mysql:8.0.12

#列出镜像
docker image ls -a

#运行容器
docker run [镜像]

下面的命令则启动一个 bash 终端，允许用户进行交互。
docker run -t -i ubuntu:18.04 /bin/bash
root@af8bae53bdd3:/#

#查找镜像
docker search [镜像]

#查看终止状态的容器
docker container ls -a

#保存容器
docker commit 【短ID】 容器名称:容器版本 

#删除容器（首先kill运行中的容器然后删除终止状态的容器）
docker kill -s KILL [短ID(终止状态)]
docker container rm 【短ID】来删除一个处于终止状态的容器
#清理所有处于终止状态的容器
docker container prune
#删除镜像
docker image rm 【短ID】

#若需要删除所有仓库名为 redis 的镜像：
docker image rm $(docker image ls -q redis)
```

### MySQL

**docker 安装 mysql 8 版本**

```
# docker 中下载 mysql
docker pull mysql:8.0.12

#启动
   docker run --name mysql  -v D:\DockerImgs\MySQLdatabase:/var/lib/mysql -p 3306:3306 -e MYSQL_ROOT_PASSWORD=root -e MYSQL_DATABASE=inst1 -d mysql:8.0.12
 
 
--name mysql  将容器命名为mysql，后面可以用这个name进行容器的启动暂停等操作
-e MYSQL_ROOT_PASSWORD=123456 设置MySQL密码为123456
-d 此容器在后台运行,并且返回容器的ID
-i 以交互模式运行容器
-p 进行端口映射，格式为主机(宿主)端口:容器端口
--restart=always 当docker重启时，该容器自动重启


#进入容器ss
docker exec -it mysql bash

#登录mysql
mysql -u root -p

#查看表
show databases;

##修改本地连接 
ALTER USER 'root'@localhost IDENTIFIED WITH mysql_native_password BY '123456';
flush privileges;
#远程连接
ALTER USER 'root'@'%' IDENTIFIED WITH mysql_native_password BY '123456';
flush privileges;

#挂载到本地（需要授权）
 -v D:\DockerImgs\MySQLdatabase:/var/lib/mysql
```

### Centos7



```
# docker 中下载 ubuntu
docker pull ubuntu:18.04

#启动且允许用户进行交互
docker run --name ubuntu ubuntu:18.04 /usr/sbin/init
docker run --privileged --name centos centos /usr/sbin/init
#进入
docker exec -it centos bash
#查看信息
root@af8bae53bdd3:/#cat /etc/os-release

#安装firewalld 防火墙
yum install firewalld

#安装ifconfig
yum search ifconfig
yum install net-tools.x86_64 -y

#设置密码
passwd    #如果没有该命令 使用yum install passwd

#安装Openssh 
yum -y install openssh-server
yum -y install openssh-clients
#修改/etc/ssh/sshd_config配置并保存：
vi /etc/ssh/sshd_config
PermitRootLogin yes    UsePAM no

port=22 #开启22端口
RSAAuthentication yes #启用 RSA 认证
PubkeyAuthentication yes #启用公钥私钥配对认证方式
AuthorizedKeysFile .ssh/authorized_keys #公钥文件路径（和上面生成的文件同）
PermitRootLogin yes #root能使用ssh登录

#启动ssh服务
systemctl start sshd
systemctl status sshd
#重启ssh服务，并设置开机启动：
yum install initscripts -y
service sshd restart
chkconfig sshd on

#安装jdk
查看yum库中的java安装包 ：yum -y list java*
yum -y install java-11-openjdk.x86_64 java-11-openjdk-devel.x86_64
#安装路径
 cd /usr/lib/jvm
 
 #安装MySQL
  yum install wget.x86_64 -y
  wget http://repo.mysql.com/mysql80-community-release-el7-1.noarch.rpm
  yum -y install mysql80-community-release-el7-1.noarch.rpm
  yum -y install mysql-community-server
  
  #查看默认密码
  grep 'temporary password' /var/log/mysqld.log
  #修改密码
  ALTER USER 'root'@'localhost' IDENTIFIED BY 'Xiao_00oo';(过于简单改不了)
  
  （
      ===#跳过密码(算了吧)
      vi /etc/my.cnf 
      配置文件添加skip-grant-tables 
      ===
  ）
  #修改远程连接
  update user set host = '%' where user = 'root';
  
  #保存后重启mysql
  systemctl restart mysqld
  
  #查看状态
  systemctl status mysqld
  
  #卸载mysql
  yum remove mysql mysql-server mysql-libs compat-mysql8
  
  #下载tomcat9
  wget http://mirror.bit.edu.cn/apache/tomcat/tomcat-9/v9.0.24/bin/apache-tomcat-9.0.24.tar.gz
  
  #解压
   tar zxvf apache-tomcat-9.0.24.tar.gz
  
  #启动 cd到安装目录 
  启动服务 ./catlina run
  关闭服务 ctrl+c
  
 #开端口命令：
 	【单个】
	firewall-cmd --zone=public --add-port=80/tcp --permanent
	【范围】
	firewall-cmd --zone=public --add-port=50000-50100/tcp --permanent
	【删除】
	firewall-cmd --zone=public --remove-port=80/tcp --permanent
 
 #重启防火墙：
     systemctl restart firewalld.service
 
 #找出公共区域的所有设置
     firewall-cmd --zone=public --list-all
     firewall-cmd --list-all
 #用于显示 tcp，udp 的端口和进程等相关情况
 	netstat -tunlp 
 #后台运行程序并把日志输出到output文件中
 nohup PORT=9999 node app.js >music.txt 2>&1 &
```

### Redis

```
# 启动Redis
	docker run -di --name myredis -p 6379:6379 redis
	docker exec -it myredis redis-cli 
	
docker run --name myredis -v C:\Users\Admin\Desktop\redis-2.4.5-win32-win64\64bit:/data -di -p 6379:6379 redis
	 -p 6379:6379 : 将容器的6379端口映射到主机的6379端口

 -d : 将容器的在后台运行

 -v $PWD/data:/data : 将主机中当前目录下的data挂载到容器的/data .redis数据卷,如未加上这个,容器重启后数据将丢失.

 redis-server --appendonly yes : 在容器执行redis-server启动命令，并打开redis持久化配置

  --requirepass "ReDis@.1*1PWD"  设置引号里字符为密码

 –restart=always : 随docker启动而启动
 
 
 
$ cd src
$  ./src/redis-server ./redis.conf
	./src/redis-cli

```

### Tomcat

```
# 启动

	docker run -it --rm -p 8888:8080 tomcat
```

### Nginx

```
# 编辑配置文件
	vim /usr/local/nginx/conf/nginx.conf
# Reload
	/usr/local/nginx/sbin/nginx -s reload
```

### MinIO Docker

```
# docker安装镜像
	docker pull minio/minio

# 启动
    docker run -p 50005:9000 -d --name minio \
      -v /var/ftp/pub/Photo/imgs/minio:/data \
      -v /mnt/config:/root/.minio \
      minio/minio server /data
# 默认用户密码 
	minioadmin
	minioadmin
```

