
要想在 MongoDB 上使用 PHP，需要使用 mongodb PHP 驱动。从[该链接处](https://s3.amazonaws.com/drivers.mongodb.org/php/index.html)下载 PHP 驱动。确保下载的是最新版。将其解压缩，并将 php_mongo.dll 文件放到 PHP 扩展目录中（默认为 “ext”），然后把下列代码放到 php.ini 文件中：  

`extension=php_mongo.dll`  


## 创建连接并选择数据库   

创建连接需要指定数据库名称，如果数据库不存在，则 mongodb 会自动创建它。  

连接数据库的代码如下所示：  

```  
<?php
// connect to mongodb
$m = new MongoClient();
echo "Connection to database successfully";
// select a database
$db = $m->mydb;
echo "Database mydb selected";
?>

```  

该程序的执行结果如下所示：  

```   
Connection to database successfully
Database mydb selected

```  



## 创建集合  

创建集合的代码如下所示：  

```  
<?php
// connect to mongodb
$m = new MongoClient();
echo "Connection to database successfully";
// select a database
$db = $m->mydb;
echo "Database mydb selected";
$collection = $db->createCollection("mycol");
echo "Collection created succsessfully";
?>

```    

执行程序的结果如下所示：  

```   
Connection to database successfully
Database mydb selected
Collection created succsessfully

```  




## 插入文档   

插入文档使用 `insert()` 方法。  

```   
<?php
// connect to mongodb
$m = new MongoClient();
echo "Connection to database successfully";
// select a database
$db = $m->mydb;
echo "Database mydb selected";
$collection = $db->mycol;
echo "Collection selected succsessfully";
$document = array( 
"title" => "MongoDB", 
"description" => "database", 
"likes" => 100,
"url" => "http://www.tutorialspoint.com/mongodb/",
"by", "tutorials point"
);
$collection->insert($document);
echo "Document inserted successfully";
?>

```   

执行程序的结果如下所示：   

```  
Connection to database successfully
Database mydb selected
Collection selected succsessfully
Document inserted successfully

```

## 寻找所有文档   

选择集合中的所有文档，使用 `find()` 方法。  

```  
<?php
// connect to mongodb
$m = new MongoClient();
echo "Connection to database successfully";
// select a database
$db = $m->mydb;
echo "Database mydb selected";
$collection = $db->mycol;
echo "Collection selected succsessfully";

$cursor = $collection->find();
// iterate cursor to display title of documents
foreach ($cursor as $document) {
echo $document["title"] . "\n";
}
?>

```   

程序执行的结果如下所示：  

```  
Connection to database successfully
Database mydb selected
Collection selected succsessfully
{
"title": "MongoDB"
}

```

## 更新文档   

更新文档使用 `update()` 方法。

下例将把插入文档的标题改为 **MongoDB Tutorial**。代码段如下所示：  

```
<?php
// connect to mongodb
$m = new MongoClient();
echo "Connection to database successfully";
// select a database
$db = $m->mydb;
echo "Database mydb selected";
$collection = $db->mycol;
echo "Collection selected succsessfully";

// now update the document
$collection->update(array("title"=>"MongoDB"), array('$set'=>array("title"=>"MongoDB Tutorial")));
echo "Document updated successfully";
// now display the updated document
$cursor = $collection->find();
// iterate cursor to display title of documents
echo "Updated document";
foreach ($cursor as $document) {
echo $document["title"] . "\n";
}
?>

```  

程序执行的结果如下所示：  

```  
Connection to database successfully
Database mydb selected
Collection selected succsessfully
Document updated successfully
Updated document
{
"title": "MongoDB Tutorial"
}

```  


## 删除文档    

删除文档使用 `remove()` 方法。  

下例中将把标题为 **MongoDB Tutorial** 的文档全部删除。代码如下所示：  

```  
<?php
// connect to mongodb
$m = new MongoClient();
echo "Connection to database successfully";
// select a database
$db = $m->mydb;
echo "Database mydb selected";
$collection = $db->mycol;
echo "Collection selected succsessfully";

// now remove the document
$collection->remove(array("title"=>"MongoDB Tutorial"),false);
echo "Documents deleted successfully";

// now display the available documents
$cursor = $collection->find();
// iterate cursor to display title of documents
echo "Updated document";
foreach ($cursor as $document) {
echo $document["title"] . "\n";
}
?>

```  

程序执行的结果如下所示：   

```  
Connection to database successfully
Database mydb selected
Collection selected succsessfully
Documents deleted successfully

```  

在上述范例中，第二个参数是布尔值类型，用于 `remove()` 方法的 `justOne` 字段。  

剩余的 mongodb 的 `findOne()`、`save()`、`limit()`、`skip()`、`sort()` 等方法将在教程剩余部分予以介绍。     





