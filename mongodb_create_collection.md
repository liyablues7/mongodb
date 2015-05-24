## `createCollection()` 方法  

在 MongoDB 中，创建集合采用 **db.createCollection(name, options)** 方法。  

### 语法格式  

`createCollection()` 方法的基本格式如下：  

`db.createCollection(name, options)`  

在该命令中，`name` 是所要创建的集合名称。`options` 是一个用来指定集合配置的文档。  

|参数|类型|描述| 
|----|----|----|  
|name|字符串|所要创建的集合名称| 
|options|文档|可选。指定有关内存大小及索引的选项|  

参数 options 是可选的，所以你必须指定的只有集合名称。下表列出了所有可用选项：   

|字段|类型|描述|
|---|---|---|
|capped|布尔|（可选）如果为 true，则创建固定集合。固定集合是指有着固定大小的集合，当达到最大值时，它会自动覆盖最早的文档。<br/>**当该值为 true 时，必须指定 size 参数。**|  
|autoIndexID|布尔|（可选）如为 true，自动创建索引》》默认为 false。|  
|size|数值|（可选）为固定集合指定一个最大值（以字节计）。<br/>**如果 capped 为 true，也需要指定该字段。**|  
|max|数值|（可选）指定固定集合中包含文档的最大数量。|  

在插入文档时，MongoDB 首先检查固定集合的 size 字段，然后检查 max 字段。  

### 范例  

不带参数的 `createCollection()` 方法的基本格式为：  

```
>use test
switched to db test
>db.createCollection("mycollection")
{ "ok" : 1 }
>

```

可以使用 `show collections` 来查看创建了的集合。

```
>show collections
mycollection
system.indexes


```

下面是带有几个关键参数的 `createCollection()` 的用法：  

```
>db.createCollection("mycol", { capped : true, autoIndexID : true, size : 6142800, max : 10000 } )
{ "ok" : 1 }
>

```

在 MongoDB 中，你不需要创建集合。当你插入一些文档时，MongoDB 会自动创建集合。  

```
>db.tutorialspoint.insert({"name" : "tutorialspoint"})
>show collections
mycol
mycollection
system.indexes
tutorialspoint
>
```















