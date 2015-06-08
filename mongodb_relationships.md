MongoDB 中的关系表示文档之间的逻辑相关方式。关系可以通过**内嵌**（Embedded）或**引用**（Referenced）两种方式建模。这样的关系可能是 1：1、1：N、N：1，也有可能是 N：N。  

先来考虑保存用户地址的例子。一个用户可能有多个地址，这是一个 1：N 的关系。  

下面是一个结构非常简单的 **user** 文档。  

```  
{
"_id":ObjectId("52ffc33cd85242f436000001"),
"name": "Tom Hanks",
"contact": "987654321",
"dob": "01-01-1991"
}

```  

下面是 **address** 文档的结构：  

```  
{
"_id":ObjectId("52ffc4a5d85242602e000000"),
"building": "22 A, Indiana Apt",
"pincode": 123456,
"city": "Los Angeles",
"state": "California"
} 

```  

## 内嵌关系的建模  

利用内嵌方法，我们将把地址文档内嵌到 user 文档中。  

```   
{
"_id":ObjectId("52ffc33cd85242f436000001"),
"contact": "987654321",
"dob": "01-01-1991",
"name": "Tom Benzamin",
"address": [
{
"building": "22 A, Indiana Apt",
"pincode": 123456,
"city": "Los Angeles",
"state": "California"
},
{
"building": "170 A, Acropolis Apt",
"pincode": 456789,
"city": "Chicago",
"state": "Illinois"
}]
} 

```  


该方法会将所有相关数据都保存在一个文档中，从而易于检索和维护。通过一个查询命令就能检索整个文档：  

`>db.users.findOne({"name":"Tom Benzamin"},{"address":1})`    

其中的 **db** 和 **users** 分别对应的是数据库和集合。  

这种方法的缺点是，如果内嵌文档不断增长，会对读写性能造成影响。   

## 引用关系的建模  

这是一种设计归一化关系的方法。按照这种方法，所有的用户和地址文档都将分别存放，而用户文档会包含一个字段，用来引用地址文档 **id** 字段。  

```  
{
"_id":ObjectId("52ffc33cd85242f436000001"),
"contact": "987654321",
"dob": "01-01-1991",
"name": "Tom Benzamin",
"address_ids": [
ObjectId("52ffc4a5d85242602e000000"),
ObjectId("52ffc4a5d85242602e000001")
]
}

```  

如上所示，user 文档包含的数组字段 **address_ids** 含有相应地址的 ObjectId 对象。利用这些 ObjectId，能够查询地址文档，从而获取地址细节信息。利用这种方法时，需要进行两种查询：首先从 **user** 文档处获取 **address_ids**，其次从 **address** 集合中获取这些地址。  

```  
>var result = db.users.findOne({"name":"Tom Benzamin"},{"address_ids":1})
>var addresses = db.address.find({"_id":{"$in":result["address_ids"]}})

```






