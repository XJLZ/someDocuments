#### 查询并显示集合中的数据

```javascript
db.getCollection('pixiv').aggregate([
    { $group: { _id : '$author.id', count: { $sum : 1 } } },
    { $match: { count: { $gt : 1} } }
])
```

#### 查询条件为数据中对象某个属性值

```javascript
db.getCollection('pixiv').find({"author.id":35562628})
```

