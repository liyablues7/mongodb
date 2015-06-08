在上一节中，我们使用**引用关系**实现了归一化的数据库结构，这种引用关系也被称作**手动引用**，即可以手动地将引用文档的 id 保存在其他文档中。但在有些情况下，文档包含其他集合的引用时，我们可以使用**数据库引用**（MongoDB DBRefs）。  

## 数据库引用 vs 手动引用  

我们将利用一个例子来展示如何用数据库引用代替手动引用。假设一个数据库中存储有多个类型的地址（家庭地址、办公室地址、邮件地址，等等），这些地址保存在不同的集合中（address_home、address_office、address_mailing，等等）。当 **user** 集合的文档引用了一个地址时，它还需要按照地址类型来指定所需要查看的集合。这种情况下，一个文档引用了许多结合的文档，所以就应该使用 DBRef。  

## 使用数据库引用  

在 DBRef 中有三个字段：  

**$ref** 该字段指定所引用文档的集合。  
**$id** 该字段指定引用文档的 `-id` 字段
**$db** 该字段是可选的，包含引用文档所在数据库的名称。  

假如在一个简单的 user 文档中包含着 DBRef 字段 **address**，如下所示：  

```  
{
"_id":ObjectId("53402597d852426020000002"),
"address": {
"$ref": "address_home",
"$id": ObjectId("534009e4d852427820000002"),
"$db": "tutorialspoint"},
"contact": "987654321",
"dob": "01-01-1991",
"name": "Tom Benzamin"
}

```


数据库引用字段 **address** 指定出，引用地址文档位于 **tutorialspoint** 数据库的 **address_home** 集合中，并且它的 id 为 534009e4d852427820000002。  


下例展示了，在由 **$ref** 所指定的集合（本例中为 **address_home**）中，如何动态查找由 **$id** 所确定的文档。  

```  
>var user = db.users.findOne({"name":"Tom Benzamin"})
>var dbRef = user.address
>db[dbRef.$ref].findOne({"_id":(dbRef.$id)})

```  

上述代码返回了 **address_home** 集合中的地址文档，如下所示：  

```  
{
"_id" : ObjectId("534009e4d852427820000002"),
"building" : "22 A, Indiana Apt",
"pincode" : 123456,
"city" : "Los Angeles",
"state" : "California"
}

```