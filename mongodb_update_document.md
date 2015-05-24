MongoDB 中的 `update()` 与 `save()` 方法都能用于更新集合中的文档。`update()` 方法更新已有文档中的值，而 `save()` 方法则是用传入该方法的文档来替换已有文档。  

## `update()` 方法  

`update()` 方法更新已有文档中的值。  

### 语法格式  

`update()` 方法基本格式如下：   

`>db.COLLECTION_NAME.update(SELECTIOIN_CRITERIA, UPDATED_DATA)`  

### 范例  

假如 mycol 集合中有下列数据：  

```
{ "_id" : ObjectId(5983548781331adf45ec5), "title":"MongoDB Overview"}
{ "_id" : ObjectId(5983548781331adf45ec6), "title":"NoSQL Overview"}
{ "_id" : ObjectId(5983548781331adf45ec7), "title":"Tutorials Point Overview"}

```
下面的例子将把文档原标题 'MongoDB Overview' 替换为新的标题 'New MongoDB Tutorial'。  


```   
>db.mycol.update({'title':'MongoDB Overview'},{$set:{'title':'New MongoDB Tutorial'}})
>db.mycol.find()
{ "_id" : ObjectId(5983548781331adf45ec5), "title":"New MongoDB Tutorial"}
{ "_id" : ObjectId(5983548781331adf45ec6), "title":"NoSQL Overview"}
{ "_id" : ObjectId(5983548781331adf45ec7), "title":"Tutorials Point Overview"}
>

```


MongoDB 默认只更新单个文档，要想更新多个文档，需要把参数 `multi` 设为 `true`。  


`>db.mycol.update({'title':'MongoDB Overview'},{$set:{'title':'New MongoDB Tutorial'}},{multi:true})`  

## `save()` 方法  

`save()` 方法利用传入该方法的文档来替换已有文档。   

### 语法格式  

`save()` 方法基本语法格式如下：  

`>db.COLLECTION_NAME.save({_id:ObjectId(),NEW_DATA})`  

### 范例  

下例用 `_id` 为 '5983548781331adf45ec7' 的文档代替原有文档。  

```  
>db.mycol.save(
{
"_id" : ObjectId(5983548781331adf45ec7), "title":"Tutorials Point New Topic", "by":"Tutorials Point"
}
)
>db.mycol.find()
{ "_id" : ObjectId(5983548781331adf45ec5), "title":"Tutorials Point New Topic", "by":"Tutorials Point"}
{ "_id" : ObjectId(5983548781331adf45ec6), "title":"NoSQL Overview"}
{ "_id" : ObjectId(5983548781331adf45ec7), "title":"Tutorials Point Overview"}
>
```  
