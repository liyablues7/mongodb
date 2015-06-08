对于衡量数据库及索引设计的效率来说，分析查询是一个很重要的衡量方式。经常使用的查询有 **$explain** 和 **$hint**。  

## 使用 $explain     

**$explain** 操作提供的消息包括：查询消息、查询所使用的索引以及其他的统计信息。在分析索引优化方案时，这是一个非常有用的工具。  

在上一节中，我们使用如下查询，针对 users 集合的字段 gender 和 user_name 创建了索引：  

`>db.users.ensureIndex({gender:1,user_name:1})`  

接下来，在下列查询中使用 **$explain**。  

`>db.users.find({gender:"M"},{user_name:1,_id:0}).explain()`  

上述 `explain()` 查询返回下列分析结果：  

```  
{
"cursor" : "BtreeCursor gender_1_user_name_1",
"isMultiKey" : false,
"n" : 1,
"nscannedObjects" : 0,
"nscanned" : 1,
"nscannedObjectsAllPlans" : 0,
"nscannedAllPlans" : 1,
"scanAndOrder" : false,
"indexOnly" : true,
"nYields" : 0,
"nChunkSkips" : 0,
"millis" : 0,
"indexBounds" : {
"gender" : [
[
"M",
"M"
]
],
"user_name" : [
[
{
"$minElement" : 1
},
{
"$maxElement" : 1
}
]
]
}
}

```  


我们将在结果集中看到这些字段：  

- **indexOnly** 的真值代表该查询使用了索引。  
- **cursor** 字段指定了游标所用的类型。BTreeCursor 类型代表了使用了索引并且提供了所用索引的名称。BasicCursor 表示进行了完整扫描，没有使用任何索引。   
- **n** 代表所返回的匹配文档的数量。  
- **nscannedObjects** 表示已扫描文档的总数。  
- **nscanned** 所扫描的文档或索引项的总数。   


## 使用 $hint


**$hint** 操作符强制索引优化器使用指定的索引运行查询。这尤其适用于测试带有多个索引的查询性能。比如，下列查询指定了用于该查询的 gender 和 user_name 字段的索引：   

`>db.users.find({gender:"M"},{user_name:1,_id:0}).hint({gender:1,user_name:1})` 


使用 $hint 来优化上述查询：  

`>db.users.find({gender:"M"},{user_name:1,_id:0}).hint({gender:1,user_name:1}).explain()`  




