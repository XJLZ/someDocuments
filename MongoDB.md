---
title: Linux安装MongoDB
date: 2020-08-10 15:32:00
tags:
- mongo
- linux
---



## 1.下载

　　官方下载地址：https://www.mongodb.com/download-center/community

## 2.上传解压

### 　　1.使用工具（FileZilla）上传至服务器

　　![img](https://i.loli.net/2020/08/10/1EluM4Qy2CVPpZ6.png)

### 　　2.解压安装

```
解压：tar -zxvf mongodb-linux-x86_64-4.0.6.tgz
移动：mv ./mongodb-linux-x86_64-4.0.6 /usr/local/mongodb
```

　　![img](https://i.loli.net/2020/12/18/glJos1c4pRftZeq.png)

## 3.配置conf与目录

### 　　1.进入mongodb目录　　　

```
cd /usr/local/mongodb/
```

　　　　![img](https://i.loli.net/2020/12/18/uta8KDdAFbHUhep.png)

### 　　2.创建db目录和日志文件　　

```
mkdir -p ./data/db
mkdir -p ./logs
touch ./logs/mongodb.log
```

　　　　![img](https://i.loli.net/2020/12/18/KJgWeotVpI2Da35.png)

### 　　3.创建mongodb.conf文件

　　　　**vim mongodb.conf**

```
#端口号
port=27017
#db目录
dbpath=/usr/local/mongodb/data/db
#日志目录
logpath=//usr/local/mongodb/logs/mongodb.log
#后台
fork=true
#日志输出
logappend=true
#允许远程IP连接
bind_ip=0.0.0.0
```

## 4.启动测试

### 　　1.启动　

```
./bin/mongod --config mongodb.conf
```

 　![img](https://i.loli.net/2020/12/18/xtuVGrRTWA1BelK.png)

### 　　2.连接　

```
./bin/mongo  --默认端口27017
./bin/mongo localhost:23234  --指定端口，需要与配置的端口号一致
```

　　![img](https://i.loli.net/2020/12/18/F8DEWIfRJ9d7C4i.png)

### 　　3.测试　　![img](https://i.loli.net/2020/12/18/xIGOgkawAnuHLlQ.png)

## 5.配置mongodb服务开机启动

###     1.设置mongodb.service开机服务启动    

```
cd /lib/systemd/system
vim mongodb.service

添加如下配置--记得路劲和自己的配置路径要一致

[Unit]
Description=mongodb
After=network.target remote-fs.target nss-lookup.target

[Service]
Type=forking
ExecStart=/usr/local/mongodb/mongodb/bin/mongod --config /usr/local/mongodb/mongodb/mongodb.conf
ExecReload=/bin/kill -s HUP $MAINPID
ExecStop=/usr/local/mongodb/mongodb/bin/mongod --shutdown --config /usr/local/mongodb/mongodb/mongodb.conf
PrivateTmp=true

[Install]
WantedBy=multi-user.target

```

 

   然后设置mongodb.service权限

```
chmod +x mongodb.service
```

​    \#启动服务 

```
systemctl start mongodb.service  
```

   \#停止服务

```
systemctl stop mongodb.service
```

   \#添加开机自启动

```
systemctl enable mongodb.service
```

   \#重启服务

```
systemctl restart mongodb.service
```

 

###   2）添加环境变量

```
1、直接用export命令：
   export  PATH=$PATH:/usr/local/mongodb/bin
2、修改profile文件：
   cat >>/etc/profile<<"EOF"
   export PATH="$PATH:/usr/local/mongodb/bin"
   EOF

   刷新profile文件：
   source /etc/profile
   
3、 修改.bashrc文件：
 	cat >>/root/.bashrc<<"EOF"
   	export PATH="$PATH:/usr/local/mongodb/bin"
   	EOF
```


   上述三步依次执行完毕，环境变量配置完成！！！！

   重启服务器，输入mongo 回车就有了。。。。。