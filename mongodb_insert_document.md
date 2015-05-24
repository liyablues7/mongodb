## `insert()` 方法  

要想将数据插入 MongoDB 集合中，需要使用 `insert()` 或 `save()` 方法。  

### 语法格式  

`insert()` 方法的基本格式为：  

`>db.COLLECTION_NAME.insert(document)`  

### 范例 1 

```
>db.mycol.insert({
_id: ObjectId(7df78ad8902c),
title: 'MongoDB Overview', 
description: 'MongoDB is no sql database',
by: 'tutorials point',
url: 'http://www.tutorialspoint.com',
tags: ['mongodb', 'database', 'NoSQL'],
likes: 100
})

```

mycol 是上一节所创建的集合的名称。如果数据库中不存在该集合，那么 MongoDB 会创建该集合，并向其中插入文档。  

在插入的文档中，如果我们没有指定 `_id` 参数，那么 MongoDB 会自动为文档指定一个唯一的 ID。   

`_id` 是一个 12 字节长的 16 进制数，这 12 个字节的分配如下：  

`_id: ObjectId(4 bytes timestamp, 3 bytes machine id, 2 bytes process id, 3 bytes incrementer)`

为了，你可以将用 `insert()` 方法传入一个文档数组，范例如下：  

### 范例 2  

```  

>db.post.insert([
{
title: 'MongoDB Overview', 
description: 'MongoDB is no sql database',
by: 'tutorials point',
url: 'http://www.tutorialspoint.com',
tags: ['mongodb', 'database', 'NoSQL'],
likes: 100
},
{
title: 'NoSQL Database', 
description: 'NoSQL database doesn't have tables',
by: 'tutorials point',
url: 'http://www.tutorialspoint.com',
tags: ['mongodb', 'database', 'NoSQL'],
likes: 20, 
comments: [	
{
user:'user1',
message: 'My first comment',
dateCreated: new Date(2013,11,10,2,35),
like: 0 
}
]
}
])

```


也可以用 `db.post.save(document)` 插入文档。如果没有指定文档的 `_id`，那么 `save()` 就和 `insert()` 完全一样了。如果指定了文档的 `_id`，那么它会覆盖掉含有 `save()` 方法中指定的 `_id`的文档的全部数据。   















