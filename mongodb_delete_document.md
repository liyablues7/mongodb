## `remove()` 方法  

MongoDB 利用 `remove()` 方法 清除集合中的文档。它有 2 个可选参数：  

- deletion criteria：（可选）删除文档的标准。  
- justOne：（可选）如果设为 true 或 1，则只删除一个文档。  

### 语法格式  

`remove()` 方法的基本语法格式如下所示：  

`>db.COLLECTION_NAME.remove(DELLETION_CRITTERIA)`  

### 范例  

假如 mycol 集合中包含下列数据：  

```   
{ "_id" : ObjectId(5983548781331adf45ec5), "title":"MongoDB Overview"}
{ "_id" : ObjectId(5983548781331adf45ec6), "title":"NoSQL Overview"}
{ "_id" : ObjectId(5983548781331adf45ec7), "title":"Tutorials Point Overview"}

```  

下面我们将删除其中所有标题为 'MongoDB Overview' 的文档。  


```  
>db.mycol.remove({'title':'MongoDB Overview'})
>db.mycol.find()
{ "_id" : ObjectId(5983548781331adf45ec6), "title":"NoSQL Overview"}
{ "_id" : ObjectId(5983548781331adf45ec7), "title":"Tutorials Point Overview"}
>

```


## 只删除一个文档  

如果有多个记录，而你只想删除第一条记录，那么就设置 `remove()` 方法中的 `justOne` 参数：  

`>db.COLLECTION_NAME.remove(DELETION_CRITERIA,1)`  


## 删除所有文档  

如果没有指定删除标准，则 MongoDB 会将集合中所有文档都予以删除。**这等同于 SQL 中的 truncate 命令**。  

```  

>db.mycol.remove()
>db.mycol.find()
>  
```  
