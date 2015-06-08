## 安装  

要想在 Java 程序中使用 MongoDB，需要先确定是否安装了 MongoDB JDBC 驱动，并且要在机器上安装了 Java。查看 Java 教程来确保在机器上安装好 Java。下面来介绍如何安装 MongoDB JDBC 驱动。  

- 从路径 Download mongo.jar 处下载 jar 文件，注意下载最新版本。  
- 在类路径中包括 mongo.jar 文件。   

## 连接数据库  

为了连接数据库，需要指定数据库名称，如果数据库不存在，mongodb 就会自动创建它。  

连接数据库的代码段如下所示：  

```   
import com.mongodb.MongoClient;
import com.mongodb.MongoException;
import com.mongodb.WriteConcern;
import com.mongodb.DB;
import com.mongodb.DBCollection;
import com.mongodb.BasicDBObject;
import com.mongodb.DBObject;
import com.mongodb.DBCursor;
import com.mongodb.ServerAddress;
import java.util.Arrays;

public class MongoDBJDBC{
public static void main( String args[] ){
try{   
// To connect to mongodb server
MongoClient mongoClient = new MongoClient( "localhost" , 27017 );
// Now connect to your databases
DB db = mongoClient.getDB( "test" );
System.out.println("Connect to database successfully");
boolean auth = db.authenticate(myUserName, myPassword);
System.out.println("Authentication: "+auth);
}catch(Exception e){
System.err.println( e.getClass().getName() + ": " + e.getMessage() );
}
}
}


```  


下面编译并运行上面的程序，以便测试数据库。可以针对每个需求改变路径。假设当前 JDBC 驱动版本 mongo-2.10.1.jar 可用于当前路径。  

```  
$javac MongoDBJDBC.java
$java -classpath ".:mongo-2.10.1.jar" MongoDBJDBC
Connect to database successfully
Authentication: true

```  


如果想使用 Windows 系统的机器，则编译并执行代码如下：   

```  
$javac MongoDBJDBC.java
$java -classpath ".;mongo-2.10.1.jar" MongoDBJDBC
Connect to database successfully
Authentication: true

```  

如果选定数据库的用户名及密码有效，则 **auth** 值为 true。   


## 创建集合  

创建集合需要使用 `com.mongodb.DB` 类的 `createCollection()` 方法。  

创建集合的代码段如下所示：  

```  
import com.mongodb.MongoClient;
import com.mongodb.MongoException;
import com.mongodb.WriteConcern;
import com.mongodb.DB;
import com.mongodb.DBCollection;
import com.mongodb.BasicDBObject;
import com.mongodb.DBObject;
import com.mongodb.DBCursor;
import com.mongodb.ServerAddress;
import java.util.Arrays;

public class MongoDBJDBC{
public static void main( String args[] ){
try{   
// To connect to mongodb server
MongoClient mongoClient = new MongoClient( "localhost" , 27017 );
// Now connect to your databases
DB db = mongoClient.getDB( "test" );
System.out.println("Connect to database successfully");
boolean auth = db.authenticate(myUserName, myPassword);
System.out.println("Authentication: "+auth);
DBCollection coll = db.createCollection("mycol");
System.out.println("Collection created successfully");
}catch(Exception e){
System.err.println( e.getClass().getName() + ": " + e.getMessage() );
}
}
}

```  

编译并执行该程序，结果如下所示：  

```  
Connect to database successfully
Authentication: true
Collection created successfully

```  


## 获取/选择一个集合  

从数据库中获取/选择一个集合，使用 `com.mongodb.DBCollection` 类的 `getCollection()` 方法。代码段如下：    

```  
import com.mongodb.MongoClient;
import com.mongodb.MongoException;
import com.mongodb.WriteConcern;
import com.mongodb.DB;
import com.mongodb.DBCollection;
import com.mongodb.BasicDBObject;
import com.mongodb.DBObject;
import com.mongodb.DBCursor;
import com.mongodb.ServerAddress;
import java.util.Arrays;

public class MongoDBJDBC{
public static void main( String args[] ){
try{   
// To connect to mongodb server
MongoClient mongoClient = new MongoClient( "localhost" , 27017 );
// Now connect to your databases
DB db = mongoClient.getDB( "test" );
System.out.println("Connect to database successfully");
boolean auth = db.authenticate(myUserName, myPassword);
System.out.println("Authentication: "+auth);
DBCollection coll = db.createCollection("mycol");
System.out.println("Collection created successfully");
DBCollection coll = db.getCollection("mycol");
System.out.println("Collection mycol selected successfully");
}catch(Exception e){
System.err.println( e.getClass().getName() + ": " + e.getMessage() );
}
}
}

```

编译并执行该程序，结果如下所示：    

```  
Connect to database successfully
Authentication: true
Collection created successfully
Collection mycol selected successfully
```  


## 插入文档  

插入文档使用 `com.mongodb.DBCollection` 类的 `insert()` 方法。代码段如下所示：  


```  
import com.mongodb.MongoClient;
import com.mongodb.MongoException;
import com.mongodb.WriteConcern;
import com.mongodb.DB;
import com.mongodb.DBCollection;
import com.mongodb.BasicDBObject;
import com.mongodb.DBObject;
import com.mongodb.DBCursor;
import com.mongodb.ServerAddress;
import java.util.Arrays;

public class MongoDBJDBC{
public static void main( String args[] ){
try{   
// To connect to mongodb server
MongoClient mongoClient = new MongoClient( "localhost" , 27017 );
// Now connect to your databases
DB db = mongoClient.getDB( "test" );
System.out.println("Connect to database successfully");
boolean auth = db.authenticate(myUserName, myPassword);
System.out.println("Authentication: "+auth);         
DBCollection coll = db.getCollection("mycol");
System.out.println("Collection mycol selected successfully");
BasicDBObject doc = new BasicDBObject("title", "MongoDB").
append("description", "database").
append("likes", 100).
append("url", "http://www.tutorialspoint.com/mongodb/").
append("by", "tutorials point");
coll.insert(doc);
System.out.println("Document inserted successfully");
}catch(Exception e){
System.err.println( e.getClass().getName() + ": " + e.getMessage() );
}
}
}

```


编译并执行该程序，结果如下所示：      

```  
Connect to database successfully
Authentication: true
Collection mycol selected successfully
Document inserted successfully

```  


## 检索所有文档    

选择集合中的所有文档，使用 `com.mongodb.DBCollection` 类的 `find()` 方法。该方法返回一个游标，所以需要迭代该游标。  

选择所有文档的代码如下所示：  

```  
import com.mongodb.MongoClient;
import com.mongodb.MongoException;
import com.mongodb.WriteConcern;
import com.mongodb.DB;
import com.mongodb.DBCollection;
import com.mongodb.BasicDBObject;
import com.mongodb.DBObject;
import com.mongodb.DBCursor;
import com.mongodb.ServerAddress;
import java.util.Arrays;

public class MongoDBJDBC{
public static void main( String args[] ){
try{   
// To connect to mongodb server
MongoClient mongoClient = new MongoClient( "localhost" , 27017 );
// Now connect to your databases
DB db = mongoClient.getDB( "test" );
System.out.println("Connect to database successfully");
boolean auth = db.authenticate(myUserName, myPassword);
System.out.println("Authentication: "+auth);         
DBCollection coll = db.getCollection("mycol");
System.out.println("Collection mycol selected successfully");
DBCursor cursor = coll.find();
int i=1;
while (cursor.hasNext()) { 
System.out.println("Inserted Document: "+i); 
System.out.println(cursor.next()); 
i++;
}
}catch(Exception e){
System.err.println( e.getClass().getName() + ": " + e.getMessage() );
}
}
}

```  

编译并执行程序的结果如下：  

```  
Connect to database successfully
Authentication: true
Collection mycol selected successfully
Inserted Document: 1
{
"_id" : ObjectId(7df78ad8902c),
"title": "MongoDB",
"description": "database",
"likes": 100,
"url": "http://www.tutorialspoint.com/mongodb/",
"by": "tutorials point"
}

```  


## 更新文档  

更新集合中的文档，使用 `com.mongodb.DBCollection` 类的 `update()` 方法。代码如下所示：   


```  
import com.mongodb.MongoClient;
import com.mongodb.MongoException;
import com.mongodb.WriteConcern;
import com.mongodb.DB;
import com.mongodb.DBCollection;
import com.mongodb.BasicDBObject;
import com.mongodb.DBObject;
import com.mongodb.DBCursor;
import com.mongodb.ServerAddress;
import java.util.Arrays;

public class MongoDBJDBC{
public static void main( String args[] ){
try{   
// To connect to mongodb server
MongoClient mongoClient = new MongoClient( "localhost" , 27017 );
// Now connect to your databases
DB db = mongoClient.getDB( "test" );
System.out.println("Connect to database successfully");
boolean auth = db.authenticate(myUserName, myPassword);
System.out.println("Authentication: "+auth);         
DBCollection coll = db.getCollection("mycol");
System.out.println("Collection mycol selected successfully");
DBCursor cursor = coll.find();
while (cursor.hasNext()) { 
DBObject updateDocument = cursor.next();
updateDocument.put("likes","200")
col1.update(updateDocument); 
}
System.out.println("Document updated successfully");
cursor = coll.find();
int i=1;
while (cursor.hasNext()) { 
System.out.println("Updated Document: "+i); 
System.out.println(cursor.next()); 
i++;
}
}catch(Exception e){
System.err.println( e.getClass().getName() + ": " + e.getMessage() );
}
}
}

```   

程序编译及运行结果如下：  

```  
Connect to database successfully
Authentication: true
Collection mycol selected successfully
Document updated successfully
Updated Document: 1
{
"_id" : ObjectId(7df78ad8902c),
"title": "MongoDB",
"description": "database",
"likes": 100,
"url": "http://www.tutorialspoint.com/mongodb/",
"by": "tutorials point"
}

```  


## 删除第一个文档  

删除集合中的第一个文档，需要使用 `findOne()` 方法选中第一个文档，然后使用 `com.mongodb.DBCollection` 类的 `remove` 方法将其删除。代码范例如下：  


```  
import com.mongodb.MongoClient;
import com.mongodb.MongoException;
import com.mongodb.WriteConcern;
import com.mongodb.DB;
import com.mongodb.DBCollection;
import com.mongodb.BasicDBObject;
import com.mongodb.DBObject;
import com.mongodb.DBCursor;
import com.mongodb.ServerAddress;
import java.util.Arrays;

public class MongoDBJDBC{
public static void main( String args[] ){
try{   
// To connect to mongodb server
MongoClient mongoClient = new MongoClient( "localhost" , 27017 );
// Now connect to your databases
DB db = mongoClient.getDB( "test" );
System.out.println("Connect to database successfully");
boolean auth = db.authenticate(myUserName, myPassword);
System.out.println("Authentication: "+auth);         
DBCollection coll = db.getCollection("mycol");
System.out.println("Collection mycol selected successfully");
DBObject myDoc = coll.findOne();
col1.remove(myDoc);
DBCursor cursor = coll.find();
int i=1;
while (cursor.hasNext()) { 
System.out.println("Inserted Document: "+i); 
System.out.println(cursor.next()); 
i++;
}
System.out.println("Document deleted successfully");
}catch(Exception e){
System.err.println( e.getClass().getName() + ": " + e.getMessage() );
}
}
}

```  

程序编译并执行结果如下所示：  

```  
Connect to database successfully
Authentication: true
Collection mycol selected successfully
Document deleted successfully

```

剩余的 mongodb 的 `save()`、`limit()`、`skip()`、`sort()` 等方法将在教程剩余部分予以介绍。   


