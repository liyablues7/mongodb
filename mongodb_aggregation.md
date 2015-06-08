聚合操作能够处理数据记录并返回计算结果。聚合操作能将多个文档中的值组合起来，对成组数据执行各种操作，返回单一的结果。它相当于 SQL 中的count(*) 和 with group by。   


## `aggregate()` 方法  

对于 MongoDB 中的聚合操作，应该使用 `aggregate()` 方法。  

### 语法格式  

`aggregate()` 方法中的基本格式如下所示：  

`>db.COLLECTION_NAME.aggregate(AGGREGATE_OPERATION)`  

### 范例  

假如某个集合包含下列数据：  

```  
{
_id: ObjectId(7df78ad8902c)
title: 'MongoDB Overview', 
description: 'MongoDB is no sql database',
by_user: 'tutorials point',
url: 'http://www.tutorialspoint.com',
tags: ['mongodb', 'database', 'NoSQL'],
likes: 100
},
{
_id: ObjectId(7df78ad8902d)
title: 'NoSQL Overview', 
description: 'No sql database is very fast',
by_user: 'tutorials point',
url: 'http://www.tutorialspoint.com',
tags: ['mongodb', 'database', 'NoSQL'],
likes: 10
},
{
_id: ObjectId(7df78ad8902e)
title: 'Neo4j Overview', 
description: 'Neo4j is no sql database',
by_user: 'Neo4j',
url: 'http://www.neo4j.com',
tags: ['neo4j', 'database', 'NoSQL'],
likes: 750
},

```  


假如想从上述集合中，归纳出一个列表，以显示每个用户写的教程数量，需要像下面这样使用 `aggregate()` 方法：  

```   
> db.mycol.aggregate([{$group : {_id : "$by_user", num_tutorial : {$sum : 1}}}])
{
"result" : [
{
"_id" : "tutorials point",
"num_tutorial" : 2
},
{
"_id" : "Neo4j",
"num_tutorial" : 1
}
],
"ok" : 1
}
>

```	

假如用 SQL 来处理上述查询，则需要使用这样的命令：`select by_user, count(*) from mycol group by by_user`。   


上例使用 **by_user** 字段来组合文档，每遇到一次 `by_user`，就递增之前的合计值。下面是聚合表达式列表。  

|表达式|描述|范例|   
|---|---|---| 
|`$sum`|对集合中所有文档的定义值进行加和操作|`db.mycol.aggregate([{$group : {_id : "$by_user", num_tutorial : {$sum : "$likes"}}}])`|
|`$avg`|对集合中所有文档的定义值进行平均值|`db.mycol.aggregate([{$group : {_id : "$by_user", num_tutorial : {$avg : "$likes"}}}])`|  
|`$min`|计算集合中所有文档的对应值中的最小值|`db.mycol.aggregate([{$group : {_id : "$by_user", num_tutorial : {$min : "$likes"}}}])`|  
|`$max`|计算集合中所有文档的对应值中的最大值|`db.mycol.aggregate([{$group : {_id : "$by_user", num_tutorial : {$max : "$likes"}}}])`|  
|`$push`|将值插入到一个结果文档的数组中|`db.mycol.aggregate([{$group : {_id : "$by_user", url : {$push: "$url"}}}])`|
|`$addToSet`|将值插入到一个结果文档的数组中，但不进行复制|`db.mycol.aggregate([{$group : {_id : "$by_user", url : {$addToSet : "$url"}}}])`|  
|`$first`|根据成组方式，从源文档中获取第一个文档。但只有对之前应用过 `$sort` 管道操作符的结果才有意义。|`db.mycol.aggregate([{$group : {_id : "$by_user", first_url : {$first : "$url"}}}])`|  
|`$last`|根据成组方式，从源文档中获取最后一个文档。但只有对之前进行过 `$sort` 管道操作符的结果才有意义。|`db.mycol.aggregate([{$group : {_id : "$by_user", last_url : {$last : "$url"}}}])`|    


## 管道的概念

在 UNIX 命令 Shell 中，管道（pipeline）概念指的是能够在一些输入上执行一个操作，然后将输出结果用作下一个命令的输入。MongoDB 的聚合架构也支持这种概念。管道中有很多阶段（stage），在每一阶段中，管道操作符都会将一组文档作为输入，产生一个结果文档（或者管道终点所得到的最终 JSON 格式的文档），然后再将其用在下一阶段。   


聚合架构中可能采取的管道操作符有：   

- **$project** 用来选取集合中一些特定字段。    
- **$match** 过滤操作。减少用作下一阶段输入的文档的数量。  
- **$group** 如上所述，执行真正的聚合操作。  
- **$sort** 对文档进行排序。   
- **$skip** 在一组文档中，跳过指定数量的文档。  
- **$limit** 将查看文档的数目限制为从当前位置处开始的指定数目。
- **$unwind** 解开使用数组的文档。当使用数组时，数据处于预连接状态，通过该操作，数据重新回归为各个单独的文档的状态。利用该阶段性操作可增加下一阶段性操作的文档数量。
