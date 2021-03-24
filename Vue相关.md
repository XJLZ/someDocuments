---
title: Vue代理
date: 2020-08-10 15:32:00
tags:
- Vue
---



#### 资源路径代理（图片等）

##### 修改index.js

```
proxyTable: {
	"/resources": {
    	target: "http://localhost:9100/", //这里填写后端存放资源文件的域名
    	ws: true,
    	changeOrigin: true, 是否跨域
    	// 如果接口中是没有api的，那就直接置空（如上）。如果接口中有api，就需要写成{‘^/api’:‘/api’}
    	pathRewrite: {
    	"/api": "/"
    }
},
```

```yml
version: '3'

services:
  febs-gateway:
    image: febs-gateway:latest
    container_name: febs-gateway
    depends_on:
      - febs-register
    volumes:
      - "/home/docker/febs/logs:/log"
    command:
      - "--nacos.url=192.168.186.140"
    ports:
      - 8301:8301
  febs-auth:
    image: febs-auth:latest
    container_name: febs-auth
    depends_on:
      - febs-register
    volumes:
      - "/home/docker/febs/logs:/log"
    command:
      - "--nacos.url=192.168.186.140"
      - "--mysql.username=root"
      - "--mysql.password=zkcmroot"
      - "--mysql.url=192.168.30.100"
      - "--redis.url=192.168.30.100"
  febs-server-system:
    image: febs-server-system:latest
    container_name: febs-server-system
    depends_on:
      - febs-register
    volumes:
      - "/home/docker/febs/logs:/log"
    command:
      - "--nacos.url=192.168.186.140"
      - "--rabbitmq.url=192.168.186.140"
      - "--rabbitmq.username=admin"
      - "--rabbitmq.password=admin"
  febs-server-test:
    image: febs-server-test:latest
    container_name: febs-server-test
    depends_on:
      - febs-register
    volumes:
      - "/home/docker/febs/logs:/log"
    command:
      - "--nacos.url=192.168.186.140"
      - "--rabbitmq.url=192.168.186.140"
      - "--rabbitmq.username=admin"
      - "--rabbitmq.password=admin"
```

