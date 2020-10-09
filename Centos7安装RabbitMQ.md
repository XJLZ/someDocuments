---
title: Linux安装RabbitMQ
date: 2020-09-22 15:40:00
tags:
- Rabbit
- 消息队列
- Centos7
---



##### 安装Erlang

RabbitMQ是用Erlang语言编写的，在本教程中我们将安装最新版本的Erlang到服务器中。 Erlang在默认的YUM存储库中不可用，因此您将需要安装EPEL存储库。 运行以下命令相同。

```
yum -y install epel-release

yum -y update
```

现在使用以下命令安装Erlang。

```
yum -y install erlang socat
```

您现在可以使用以下命令检查Erlang版本。

erl -version

您将得到以下输出。

```
[root@liptan-pc ~]# erl -version
Erlang (ASYNC_THREADS,HIPE) (BEAM) emulator version 5.10.4
```

要切换到Erlang [shell](https://www.linuxcool.com/)，可以键入以下命令。

```
erl
```

shell将更改，您将得到以下输出。

```
Erlang R16B03-1 (erts-5.10.4) [source] [64-bit] [async-threads:10] [hipe] [kernel-poll:false]

Eshell V5.10.4  (abort with ^G)
1>
```

您可以通过按ctrl + C两次退出shell。 Erlang现在安装在系统上，现在可以继续安装RabbitMQ。

##### 安装RabbitMQ

RabbitMQ为预编译并可以直接安装的企业[Linux系统](https://www.linuxprobe.com/)提供RPM软件包。 唯一需要的依赖是将Erlang安装到系统中。 我们已经安装了Erlang，我们可以进一步下载RabbitMQ。 通过运行下载Erlang RPM软件包。

```
wget https://www.rabbitmq.com/releases/rabbitmq-server/v3.6.10/rabbitmq-server-3.6.10-1.el7.noarch.rpm
```

如果你没有安装wget ，可以运行yum -y install wget 。 您可以随时找到最新版本的RabbitMQ下载页面的链接。

通过运行导入GPG密钥：

```
rpm –import https://www.rabbitmq.com/rabbitmq-release-signing-key.asc
```

运行RPM安装RPM包：

```
rpm -Uvh rabbitmq-server-3.6.10-1.el7.noarch.rpm
```

RabbitMQ现已安装在您的系统上。

##### 开始RabbitMQ

您可以通过运行以下命令启动RabbitMQ服务器进程。

```
systemctl start rabbitmq-server
```

要在引导时自动启动RabbitMQ，请运行以下命令。

```
systemctl enable rabbitmq-server
```

要检查RabbitMQ服务器的状态，请运行：

```
systemctl status rabbitmq-server
```

如果启动成功，您应该得到以下输出。

```
? rabbitmq-server.service - RabbitMQ broker
   Loaded: loaded (/usr/lib/systemd/system/rabbitmq-server.service; enabled; vendor preset: disabled)
   Active: active (running) since Sat 2017-07-15 18:59:14 UTC; 3min 22s ago
 Main PID: 29006 (beam.smp)
   Status: "Initialized"
   CGroup: /system.slice/rabbitmq-server.service
           ??29006 /usr/lib64/erlang/erts-9.0/bin/beam.smp -W w -A 64 -P 1048576 -t 5000000 -stbt db -zdbbl 32000 -K tr...
           ??29149 /usr/lib64/erlang/erts-9.0/bin/epmd -daemon
           ??29283 erl_child_setup 1024
           ??29303 inet_gethost 4
           ??29304 inet_gethost 4

Jul 15 18:59:13 centos rabbitmq-server[29006]: Starting broker...
Jul 15 18:59:14 centos rabbitmq-server[29006]: systemd unit for activation check: "rabbitmq-server.service"
Jul 15 18:59:14 centos systemd[1]: Started RabbitMQ broker.
Jul 15 18:59:14 centos rabbitmq-server[29006]: completed with 0 plugins.
```

##### 修改防火墙和SE[Linux](https://www.linuxprobe.com/)规则

如果您已安装并运行防火墙 ，则必须通过防火墙允许端口8161。 运行以下命令相同。

或者直接关闭防火墙  **systemctl stop firewalld.service**

```
firewall-cmd –zone=public –permanent –add-port=4369/tcp
firewall-cmd –zone=public –permanent –add-port=25672/tcp
firewall-cmd –zone=public –permanent –add-port=5671-5672/tcp
firewall-cmd –zone=public –permanent –add-port=15672/tcp
firewall-cmd –zone=public –permanent –add-port=61613-61614/tcp
firewall-cmd –zone=public –permanent –add-port=1883/tcp
firewall-cmd –zone=public –permanent –add-port=8883/tcp
firewall-cmd –reload
```

如果您启用SELinux，则必须运行以下命令以允许RabbitMQ服务。

```
setsebool -P nis_enabled 1
```

##### 访问Web控制台

启动RabbitMQ Web管理控制台，方法是运行：

```
rabbitmq-plugins enable rabbitmq_management
```

通过运行以下命令，将RabbitMQ文件的所有权提供给RabbitMQ用户：

```
chown -R rabbitmq:rabbitmq /var/lib/rabbitmq/
```

现在，您将需要为RabbitMQ Web管理控制台创建管理用户。 运行以下命令相同。

```
rabbitmqctl add_user admin StrongPassword
rabbitmqctl set_user_tags admin administrator
rabbitmqctl set_permissions -p / admin “.*” “.*” “.*”
修改密码
rabbitmqctl  change_password  admin  'Newpassword'
```

将管理员更改为管理员用户的首选用户名。 确保将StrongPassword更改为非常强大的密码。

要访问RabbitMQ的管理面板，请使用您最喜爱的Web浏览器并打开以下URL。

```
http://Your_Server_IP:15672/
```

##### 开启远程访问 

在使用上述新建的账号访问webUI界面可以看到Config file，在overview下面, 如果没有找到配置文件后面会加上（not found）

![image-20200922163249512](https://i.loli.net/2020/09/22/Q1dIyYVlfcCTBJR.png)

没有就到该目录下新建一个，并开启远程访问（在结合SpringBoot时，RabbitMQ初始化默认是使用guest账户，因此需要给该账户开启远程访问），配置如下

```
[                                                                                                           { rabbit , [ { tcp_listeners , [ 5672 ] } , { loopback_users , [ "guest" ] } ] }
].
```

**ps：最后的英文句号不能漏掉**