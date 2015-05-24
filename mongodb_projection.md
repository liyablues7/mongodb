
在 MongoDB 中，映射（Projection）指的是只选择文档中的必要数据，而非全部数据。如果文档有 5 个字段，而你只需要显示 3 个，则只需选择 3 个字段即可。


## `find()` 方法  

MongoDB 的查询文档曾介绍过 `find()` 方法，它可以利用 AND 或 OR 条件来获取想要的字段列表。在 MongoDB 中执行 `find()` 方法时，显示的是一个文档的所有字段。要想限制，可以利用 0 或 1 来设置字段列表。1 用于显示字段，0 用于隐藏字段。  

### 语法格式  

带有映射的 `find()` 方法的基本语法格式为：  

`>db.COLLECTION_NAME.find({},{KEY:1})`  

### 范例  

假如 mycol 集合拥有下列数据：  

```

{ "_id" : ObjectId(5983548781331adf45ec5), "title":"MongoDB Overview"}
{ "_id" : ObjectId(5983548781331adf45ec6), "title":"NoSQL Overview"}
{ "_id" : ObjectId(5983548781331adf45ec7), "title":"Tutorials Point Overview"}

```

下例将在查询文档时显示文档标题。  

```  
>db.mycol.find({},{"title":1,_id:0})
{"title":"MongoDB Overview"}
{"title":"NoSQL Overview"}
{"title":"Tutorials Point Overview"}
>

```  

注意：在执行 `find()` 方法时，`_id` 字段是一直显示的。如果不想显示该字段，则可以将其设为 0。  

