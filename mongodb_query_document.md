## `find()` 方法  

要想查询 MongoDB 集合中的数据，使用 `find()` 方法。  

### 语法格式    

`find()` 方法的基本格式为：  

`>db.COLLECTION_NAME.find()`  

`find()` 方法会以非结构化的方式来显示所有文档。  


## `pretty()` 方法   

用格式化方式显示结果，使用的是 `pretty()` 方法。  

### 语法格式  

`>db.mycol.find().pretty()`     

### 范例  

```  
>db.mycol.find().pretty()
{
"_id": ObjectId(7df78ad8902c),
"title": "MongoDB Overview", 
"description": "MongoDB is no sql database",
"by": "tutorials point",
"url": "http://www.tutorialspoint.com",
"tags": ["mongodb", "database", "NoSQL"],
"likes": "100"
}
>

```

除了 `find()` 方法之外，还有一个 `findOne()` 方法，它只返回一个文档。  

## MongoDB 中类似于 WHERE 子句的语句  

如果想要基于一些条件来查询文档，可以使用下列操作。  

|操作|格式|范例|RDBMS中的类似语句|
|----|----|----|----|  
|等于|`{<key>:<value>`}|`db.mycol.find({"by":"tutorials point"}).pretty()`|`where by = 'tutorials point'`|  
|小于|`{<key>:{$lt:<value>}}`|`db.mycol.find({"likes":{$lt:50}}).pretty()`|`where likes < 50`|  
|小于或等于|`{<key>:{$lte:<value>}}`|`db.mycol.find({"likes":{$lte:50}}).pretty()`|`where likes <= 50`|
|大于|`{<key>:{$gt:<value>}}`|`db.mycol.find({"likes":{$gt:50}}).pretty()`|`where likes > 50`|
|大于或等于|`{<key>:{$gte:<value>}}`|`db.mycol.find({"likes":{$gte:50}}).pretty()`|`where likes >= 50`|  
|不等于|`{<key>:{$ne:<value>}}`|`db.mycol.find({"likes":{$ne:50}}).pretty()`|`where likes != 50`|  


## MongoDB 中的 And 条件 

### 语法格式    

在 `find()` 方法中，如果传入多个键，并用逗号(`,`)分隔它们，那么 MongoDB 会把它看成是 **AND** 条件。AND 条件的基本语法格式为：  

`>db.mycol.find({key1:value1, key2:value2}).pretty()`  

### 范例  

下例将展示所有由 “tutorials point” 发表的标题为 “MongoDB Overview” 的教程。  

```   
>db.mycol.find({"by":"tutorials point","title": "MongoDB Overview"}).pretty()
{
"_id": ObjectId(7df78ad8902c),
"title": "MongoDB Overview", 
"description": "MongoDB is no sql database",
"by": "tutorials point",
"url": "http://www.tutorialspoint.com",
"tags": ["mongodb", "database", "NoSQL"],
"likes": "100"
}
>

```  

对于上例这种情况，RDBMS 采用的 WHERE 子句将会是：**where by='tutorials point' AND title='MongoDB Overview'**。你可以在 find 子句中传入任意的键值对。  

## MongoDB 中的 OR 条件   

### 语法格式  

若基于 **OR** 条件来查询文档，可以使用关键字 **$or**。 OR 条件的基本语法格式为：  

```  
>db.mycol.find(
{
$or: [
{key1: value1}, {key2:value2}
]
}
).pretty()

```

### 范例  

下例将展示所有由 “tutorials point” 发表的标题为 “MongoDB Overview” 的教程。      

```  
>db.mycol.find({$or:[{"by":"tutorials point"},{"title": "MongoDB Overview"}]}).pretty()
{
"_id": ObjectId(7df78ad8902c),
"title": "MongoDB Overview", 
"description": "MongoDB is no sql database",
"by": "tutorials point",
"url": "http://www.tutorialspoint.com",
"tags": ["mongodb", "database", "NoSQL"],
"likes": "100"
}
>

```


## 结合使用 AND 与 OR 条件  

### 范例  

下例所展示文档的条件为：喜欢数大于 100，标题是 “MongoDB Overview”，或者是由 “tutorials point” 所发表的。响应的 SQL WHERE 子句为：**where likes>10 AND (by = 'tutorials point' OR title = 'MongoDB Overview')**  

```   
>db.mycol.find({"likes": {$gt:10}, $or: [{"by": "tutorials point"},{"title": "MongoDB Overview"}]}).pretty()
{
"_id": ObjectId(7df78ad8902c),
"title": "MongoDB Overview", 
"description": "MongoDB is no sql database",
"by": "tutorials point",
"url": "http://www.tutorialspoint.com",
"tags": ["mongodb", "database", "NoSQL"],
"likes": "100"
}
>

```


