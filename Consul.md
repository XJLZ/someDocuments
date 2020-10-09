---
title: Consul
date: 2020-08-10 15:32:00
tags:
- Centos7
- Consul
---

#### 安装

1.下载

[Consul]: https://releases.hashicorp.com/consul/1.8.3/consul_1.8.3_linux_amd64.zip

```
wget https://releases.hashicorp.com/consul/1.8.3/consul_1.8.3_linux_amd64.zip 
curl https://releases.hashicorp.com/consul/1.8.3/consul_1.8.3_linux_amd64.zip > consul.zip
```

**ps:下载过慢请试着直接浏览器下载，再用FTP工具上传至服务器**



2.解压

```
1.安装unzip
	yum -y install zip unzip
2.解压
	unzip consul.zip
```



#### ui界面启动

```
./consul agent -dev  -client 0.0.0.0 -ui
访问 IP:8500
记得开端口，或者把防火墙关闭
```

