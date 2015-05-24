## `use` 命令  

MongoDB 用 `use` + **数据库名称** 的方式来创建数据库。`use` 会创建一个新的数据库，如果该数据库存在，则返回这个数据库。  

### 语法格式   

`use` 语句的基本格式如下：  

`use DATABASE_NAME`  

### 范例  

创建一个名为 **<mydb>** 的数据库，使用 `use` 语句如下：  

```
>use mydb
switched to db mydb

```

使用命令 `db` 检查当前选定的数据库。  

```
>db
mydb

```

使用命令 `show dbs` 来检查数据库列表。  

```
>show dbs
local     0.78125GB
test      0.23012GB

```  

刚创建的数据库（mydb）没有出现在列表中。为了让数据库显示出来，至少应该插入一个文档。

```
>db.movie.insert({"name":"tutorials point"})
>show dbs
local      0.78125GB
mydb       0.23012GB
test       0.23012GB

```


在 MongoDB 中，默认的数据库是 test，如果你没有创建任何数据库，那么集合就会保存在 test 数据库中。