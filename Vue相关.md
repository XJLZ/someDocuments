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

