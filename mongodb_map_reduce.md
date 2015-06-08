在 MongoDB 文档中，Map-Reduce（映射归约）是一种将大量数据压缩成有用的聚合结果的数据处理范式。MongoDB 使用 **mapReduce** 命令来实现映射归约操作。映射归约通常用来处理大型数据。  

## 映射归约命令  

**mapReduce** 命令的基本格式为：  

```  
>db.collection.mapReduce(
function() {emit(key,value);},  //map function
function(key,values) {return reduceFunction},   //reduce function
{
out: collection,
query: document,
sort: document,
limit: number
}
)

```


mapReduce 函数首先查询集合，然后将结果文档利用 emit 函数映射为键值对，然后再根据有多个值的键来简化。  

上述语法格式中：  

- **map** 一个 JavaScript 函数，将一个值与键对应起来，并生成键值对。  
- **reduce** 一个 JavaScript 函数，用来减少或组合所有拥有同一键的文档。 
- **out** 指定映射归约查询结果的位置。  
- **query** 指定选择文档所用的选择标准（可选的）。  
- **sort** 指定可选的排序标准。  
- **limit** 指定返回的文档的最大数量值（可选的）。  

## 使用 mapReduce   

以下面这个存储用户发帖的文档结构。该文档存储用户的用户名（user_name）和发帖状态（status）。

```  
{
"post_text": "tutorialspoint is an awesome website for tutorials",
"user_name": "mark",
"status":"active"
}

``` 

在 **posts** 集合上使用 mapReduce 函数选择所有的活跃帖子，将它们基于用户名组合起来，然后计算每个用户的发帖量。代码如下：  

```  
>db.posts.mapReduce( 
function() { emit(this.user_id,1); }, 
function(key, values) {return Array.sum(values)}, 
{  
query:{status:"active"},  
out:"post_total" 
}
)

```  

上面的 mapReduce 查询输出结果如下：  

```
{
"result" : "post_total",
"timeMillis" : 9,
"counts" : {
"input" : 4,
"emit" : 4,
"reduce" : 2,
"output" : 2
},
"ok" : 1,
}

```  


结果显示，只有 4 个文档符合查询条件（`status:"active"`），于是 `map` 函数就生成了 4 个带有键值对的文档，而最终 `reduce` 函数将具有相同键值的映射文档变为了 2 个。  

要想查看 `mapReduce` 查询的结果，使用 `find` 操作符。  

```  
>db.posts.mapReduce( 
function() { emit(this.user_id,1); }, 
function(key, values) {return Array.sum(values)}, 
{  
query:{status:"active"},  
out:"post_total" 
}
).find()

```


上述查询的结果如下，显示出用户 tom 和 mark 都发了 2 个活跃的帖子。  

```  
{ "_id" : "tom", "value" : 2 }
{ "_id" : "mark", "value" : 2 }

```  

MapReduce 查询同样也可以用来构建大型复杂的聚合查询，自定义 JavaScript 函数使得 MapReduce 更为灵活与强大。  


