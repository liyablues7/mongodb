## `limit()` 方法  

要想限制 MongoDB 中的记录，可以使用 `limit()` 方法。`limit()` 方法接受一个数值类型的参数，其值为想要显示的文档数。  

### 语法格式  

`limit()` 方法的基本语法格式为：  

`>db.COLLECTION_NAME.find().limit(NUMBER)`  

### 范例  

假设 mycol 集合拥有下列数据：  

```
{ "_id" : ObjectId(5983548781331adf45ec5), "title":"MongoDB Overview"}
{ "_id" : ObjectId(5983548781331adf45ec6), "title":"NoSQL Overview"}
{ "_id" : ObjectId(5983548781331adf45ec7), "title":"Tutorials Point Overview"}

```  

下例将在查询文档时只显示 2 个文档。  

```   
>db.mycol.find({},{"title":1,_id:0}).limit(2)
{"title":"MongoDB Overview"}
{"title":"NoSQL Overview"}
>

```

如果未指定 `limit()` 方法中的数值参数，则将显示该集合内的所有文档。  

## `skip()` 方法  

### 语法格式  

`skip()` 方法基本语法格式为：  

`>db.COLLECTION_NAME.find().limit(NUMBER).skip(NUMBER)`  

### 范例  

下例将只显示第二个文档：  

```
>db.mycol.find({},{"title":1,_id:0}).limit(1).skip(1)
{"title":"NoSQL Overview"}
>

```

注意：`skip()` 方法中的默认值为 0。   
