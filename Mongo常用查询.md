---
title: Mongo常用查询
date: 2020-08-10 15:32:00
tags:
- mongodb
---



#### 查询并显示集合中的数据

```javascript
db.getCollection('pixiv').aggregate([
    { $group: { _id : '$author.id', count: { $sum : 1 } } },
    { $match: { count: { $gt : 1} } }
])
```

#### 查询条件为数据中对象某个属性值 2333

```javascript
db.getCollection('pixiv').find({"author.id":35562628})
```

#### 命令导出数据库

```
./mongoexport -h 127.0.0.1 --port 50003 -d Images -c picjson -o /home/picjson.js --type json -f "_id,tags,pid,p,uid,title,author,url,r18,width,height,_v"
```

#### 命令导入数据库

```
./mongoimport -h 127.0.0.1 --port 50003 -d Pixiv -c picjson --file /home/picjson.js --type json
```

