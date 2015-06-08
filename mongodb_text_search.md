MongoDB 从 2.4 版本起就开始支持全文索引，以便搜索字符串内容。**文本搜索**使用字干搜索技术查找字符串字段中的指定词语，丢弃字干停止词（比如 a、an、the等）。迄今为止，MongoDB 支持大约 15 种语言。  

## 启用文本搜索  

最初的文本搜索只是一种试验性功能，但从 2.6 版本起就成为默认功能了。但如果使用的是之前的 MongoDB，则需要使用下列代码启用文本搜索：  

`>db.adminCommand({setParameter:true,textSearchEnabled:true})`    

## 创建文本索引  

假设在 **posts** 集合中的下列文档中含有帖子文本及其标签。  

```  
{
"post_text": "enjoy the mongodb articles on tutorialspoint",
"tags": [
"mongodb",
"tutorialspoint"
]
}

```  

我们将在 `post_text` 字段上创建文本索引，以便搜索帖子文本之内的内容。  

`>db.posts.ensureIndex({post_text:"text"})`  

## 使用文本索引  

现在我们已经在 `post_text` 字段上创建了文本索引，接下来搜索包含 **tutorialspoint** 文本内容的帖子。  

`>db.posts.find({$text:{$search:"tutorialspoint"}})`   

在返回的结果文档中，果然包含了具有 **tutorialspoint** 文本的帖子文本。 
```  
{ 
"_id" : ObjectId("53493d14d852429c10000002"), 
"post_text" : "enjoy the mongodb articles on tutorialspoint", 
"tags" : [ "mongodb", "tutorialspoint" ]
}
{
"_id" : ObjectId("53493d1fd852429c10000003"), 
"post_text" : "writing tutorials on mongodb",
"tags" : [ "mongodb", "tutorial" ] 
}

```  

如果用的是旧版本的 MongoDB，则需使用下列代码：  

`>db.posts.runCommand("text",{search:" tutorialspoint "})`   

总之，与普通搜索相比，使用文本搜索可极大地改善搜索效率。  

## 删除文本索引  

要想删除已有的文本索引，首先要找到索引名称：  

`>db.posts.getIndexes()`  

从上述查询中获取了索引名称后，运行下列命令，**post_text_text** 是我们需要删掉的索引名称。  

`>db.posts.dropIndex("post_text_text")` 





