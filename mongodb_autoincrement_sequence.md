MongoDB 没有提供类似 SQL 数据库所具有的自动增长功能（auto-increment）。默认情况下，MongoDB 将 **_id** 字段（使用 12 字节的 ObjectId）来作为文档的唯一标识。但在有些情况下，我们希望 _id 字段值能够自动增长，而不是固守在 ObjectId 值上。  

由于这不是 MongoDB 的默认功能，所以我们按照 MongoDB 文档所建议的方式，使用 **counters** 集合来程序化地实现该功能。  

## 使用 `counters` 集合  

假设存在下列文档 **products**，我们希望 _id 字段值是一个**能够自动增长的整数序列**（1、2、3、4 …… n）。  

```  
{
  "_id":1,
  "product_name": "Apple iPhone",
  "category": "mobiles"
}

```  

创建一个 `counters` 集合，其中包含了所有序列字段最后的序列值。  

`>db.createCollection("counters")`  

现在，将下列文档（**productid** 是它的键）插入到 `counters` 集合中：  

```  
{
  "_id":"productid",
  "sequence_value": 0
}

```  

**sequence_value** 字段保存了序列的最后值。  

使用下列命令将序列文档插入到 `counters` 集合中。  

`>db.counters.insert({_id:"productid",sequence_value:0})`   


## 创建 JavaScript 函数  

接着，我们创建一个 `getNextSequenceValue` 函数，该函数以序列名为输入，按照 1 的幅度增加序列数，返回更新的序列数。在该例中，序列名称为 **productid**。  

```  
>function getNextSequenceValue(sequenceName){
   var sequenceDocument = db.counters.findAndModify(
      {
         query:{_id: sequenceName },
         update: {$inc:{sequence_value:1}},
         new:true
      });
   return sequenceDocument.sequence_value;
}

```   

## 使用 JavaScript 函数  

下面，在创建新文档以及将返回的序列值赋予文档的 _id 字段过程中，我们使用 `getNextSequenceValue` 函数。  

使用下列代码插入两个范例文档：  


```  
>db.products.insert({
   "_id":getNextSequenceValue("productid"),
   "product_name":"Apple iPhone",
   "category":"mobiles"})

>db.products.insert({
   "_id":getNextSequenceValue("productid"),
   "product_name":"Samsung S3",
   "category":"mobiles"})

```  

如上所示，使用 `getNextSequenceValue` 函数为 _id 字段设定值。

为了验证该功能，使用 find 命令来获取文档：  

`>db.prodcuts.find()`  

在上面的查询返回的结果文档中，_id 字段值果然是自动增长的：   

```  
{ "_id" : 1, "product_name" : "Apple iPhone", "category" : "mobiles"}

{ "_id" : 2, "product_name" : "Samsung S3", "category" : "mobiles" }

```

