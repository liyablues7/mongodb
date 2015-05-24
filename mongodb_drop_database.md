## `dropDatabase()` 方法    

MongoDB 的 `dropDatabase()` 命令用于删除已有数据库。  

### 语法格式  

`dropDatabase()` 命令的语法格式如下：  

`db.dropDatabase()`  

它将删除选定的数据库。如果没有选定要删除的数据库，则它会将默认的 test 数据库删除。  

### 范例  

首先使用 `show dbs` 来列出已有的数据库。  

```
>show dbs
local      0.78125GB
mydb       0.23012GB
test       0.23012GB
>
```

如果想删除新数据库 **<mydb>**，如下面这样使用 `dropDatabase()` 方法：  

```  

>use mydb
switched to db mydb
>db.dropDatabase()
>{ "dropped" : "mydb", "ok" : 1 }
>

```


再来看一下数据库列表，确实删除了 <mydb>。  

```  
>show dbs
local      0.78125GB
test       0.23012GB
>
```  















