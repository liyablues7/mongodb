前面的几章中都涉及到了 MongoDB 的对象 id。本章将介绍 ObjectId 的结构。  

**ObjectId** 是一个 12 字节的 BSON 类型，其结构如下：  

- 前 4 个字节代表 UNIX 的时间戳（以秒计）。  
- 接下来的 3 个字节代表**机器标识符**。  
- 接下来的 2 个字节代表**进程 id**。  
- 最后 3 个字节代表**随机数**。  

MongoDB 使用 ObjectId 作为每一文档的 **_id** 字段的默认值（在创建文档时产生）。ObjectId 的复杂组合保证了 _id 字段的唯一性。  

## 创建新的 ObjectId  

使用下列代码创建新的 ObjectId：  

`>newObjectId = ObjectId()`  

上述代码返回一个唯一的 id：  

`ObjectId("5349b4ddd2781d08c09890f3")`  

如果不用 MongoDB 来生成，可以自己提供一个 12 字节的 id：  

`>myObjectId = ObjectId("5349b4ddd2781d08c09890f4")`  

## 创建文档时间戳  

因为 _id ObjectId 默认保存 4 字节的时间戳，所以在大多数情况下不需要保存文档的创建时间。使用 `getTimestamp` 方法可获取文档的创建时间。  

`>ObjectId("5349b4ddd2781d08c09890f4").getTimestamp()`  

返回标准时期格式表示的文档创建时间：  

`ISODate("2014-04-12T21:49:17Z")`  

## 将 ObjectId 转换为字符串  

在某些情况下，你需要用字符串格式表示 ObjectId 值，使用如下命令可实现这一点：  

`>newObjectId.str`  

上述代码以 GUID 格式返回字符串。

`5349b4ddd2781d08c09890f3`    


