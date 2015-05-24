MongoDB 是一款跨平台、面向文档的数据库。用它创建的数据库可以实现高性能、高可用性，并且能够轻松扩展。MongoDB 的运行方式主要基于两个概念：集合（collection）与文档（document）。  

## 数据库  

数据库是集合的实际容器。每一数据库都在文件系统中有自己的一组文件。一个 MongoDB 服务器通常有多个数据库。  


## 集合  

集合就是一组 MongoDB 文档。它相当于关系型数据库（RDBMS）中的表这种概念。集合位于单独的一个数据库中。集合不能执行模式（schema）。一个集合内的多个文档可以有多个不同的字段。一般来说，集合中的文档都有着相同或相关的目的》》。  

## 文档  

文档就是一组键-值对。文档有着动态的模式，这意味着同一集合内的文档不需要具有同样的字段或结构。集合文档的   

下表展示了关系型数据库与 MongoDB 在术语上的对比：   

|关系型数据库|MongoDB|  
|---|---|  
|数据库|数据库|  
|表|集合|  
|行|文档|  
|列|字段|  
|表 Join|内嵌文档| 
|主键|主键（由 MongoDB 提供的默认 key_id）|  

|数据库服务器|客户端|
|---|---|
|MySQL/Oracle|MongoDB|  
|mysql/sqlplus|mongo|


## 范例文档  

下面这个范例展示了一个简单的博客站点的文档结构，它是由逗号分隔的键值对构成的。  

```
{
_id: ObjectId(7df78ad8902c)
title: 'MongoDB Overview', 
description: 'MongoDB is no sql database',
by: 'tutorials point',
url: 'http://www.tutorialspoint.com',
tags: ['mongodb', 'database', 'NoSQL'],
likes: 100, 
comments: [	
{
user:'user1',
message: 'My first comment',
dateCreated: new Date(2011,1,20,2,15),
like: 0 
},
{
user:'user2',
message: 'My second comments',
dateCreated: new Date(2011,1,25,7,45),
like: 5
}
]
}     
```

**_id** 是一个 12 字节长的十六进制数，它保证了每一个文档的唯一性。在插入文档时，需要提供 `_id`。如果你不提供，那么 MongoDB 就会为每一文档提供一个唯一的 id。`_id` 的头 4 个字节代表的是当前的时间戳，接着的后 3 个字节表示的是机器 id 号，接着的 2 个字节表示 MongoDB 服务器进程 id，最后的 3 个字节代表递增值。   
