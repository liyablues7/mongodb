## `drop()` 方法  

MongoDB 利用 `db.collection.drop()` 来删除数据库中的集合。  

### 语法格式  

`drop()` 命令的基本格式如下：  

`db.COLLECTION_NAME.drop()` 

### 范例  

首先检查在数据库 mydb 中已有集合：  

```  
>use mydb
switched to db mydb
>show collections
mycol
mycollection
system.indexes
tutorialspoint
>

```  

接着删除集合 mycollection。  

```  
>db.mycollection.drop()
true
>

```

再次检查数据库中的现有集合：  

```
>show collections
mycol
system.indexes
tutorialspoint
>

```

如果成功删除选定集合，则 `drop()` 方法返回 true，否则返回 false。   











