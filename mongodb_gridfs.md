
## GridFS 简介  

GridFS 是 MongoDB 的一个用来存储/获取大型数据（图像、音频、视频等类型的文件）的规范。它相当于一个存储文件的文件系统，但它的数据存储在 MongoDB 的集合中。GridFS 能存储超过文档尺寸限制（16 MB）的文件。  

GridFS 将文件分解成块，将每块数据保存在不同的文档中，每块大小最高为 255 KB。  

GridFS 默认使用 **fs.files** 和 **fs.chunks** 集合来存储文件的元数据和块。每个块都由唯一的 `_id` ObjectId 字段来标识。fs.files 用作父级文档。**fs.chunks** 文档中的 **files_id** 字段将块连接到父级文档上。     

fs.files 集合的一个范例文档如下所示：  

```  
{
"filename": "test.txt",
"chunkSize": NumberInt(261120),
"uploadDate": ISODate("2014-04-13T11:32:33.557Z"),
"md5": "7b762939321e146569b07f72c62cca4f",
"length": NumberInt(646)
}

```    

该文档指定了文件名、块大小、上传日期以及长度。  

下面是 fs.chunks 文档的一个范例文档：  

```   
{
"files_id": ObjectId("534a75d19f54bfec8a2fe44b"),
"n": NumberInt(0),
"data": "Mongo Binary Data"
}

```  


## 为 GridFS 添加文件    

下面我们将使用 **put** 命令，利用 GridFS 存储一个 mp3 文件。该例中，我们将使用 MongoDB 的 bin 文件夹下的 **mongofiles.exe** 工具。   

打开命令行提示符，浏览至 mongofiles.exe，输入下列代码：  

`>mongofiles.exe -d gridfs put song.mp3`    

上述代码中，**gridfs** 是保存文件所在的数据库名称。如果数据库不存在，MongoDB 将自动创建一个新数据库，Song.mp3 是上传文件名。要想查看数据库中文件的文档，使用 `find()` 查询：    

`>db.fs.files.find()`    

返回结果文档如下所示：  

```  
{
_id: ObjectId('534a811bf8b4aa4d33fdf94d'), 
filename: "song.mp3", 
chunkSize: 261120, 
uploadDate: new Date(1397391643474), md5: "e4f53379c909f7bed2e9d631e15c1c41",
length: 10401959 
}

```  

我们还可以查看 **fs.chunks** 集合中所有与存储文件相关的块。代码如下，其中使用了上次查询所返回的文档 id。  

`>db.fs.chunks.find({files_id:ObjectId('534a811bf8b4aa4d33fdf94d')})`    

该例中，查询返回了 40 个文档，这就是说整个 Mp3 文件被分解成了 40 个数据块。   

