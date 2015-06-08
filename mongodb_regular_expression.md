正则表达式在所有语言当中都是经常会用到的一个功能，可以用来搜索模式或字符串中的单词。MongoDB 也提供了这一功能，使用 **$regex** 运算符来匹配字符串模式。MongoDB 使用 PCRE（可兼容 Perl 的正则表达式）作为正则表达式语言。  

与文本搜索不同，使用正则表达式不需要使用任何配置或命令。  

假如 **posts** 集合有下面这个文档，它包含着帖子文本及其标签。  

```  
{
"post_text": "enjoy the mongodb articles on tutorialspoint",
"tags": [
"mongodb",
"tutorialspoint"
]
}

```  

## 使用正则表达式  

使用下列正则表达式来搜索包含 **tutorialspoint** 的所有帖子。   

`>db.posts.find({post_text:{$regex:"tutorialspoint"}})`  

同样的查询也可以写作下列形式：  

`>db.posts.find({post_text:/tutorialspoint/})`  


## 使用不区分大小写的正则表达式  

要想使搜索不区分大小写，使用 `$options` 参数和值 `$i`。下列命令将搜索包含 “tutorialspoint” 的字符串，不区分大小写。  

`>db.posts.find({post_text:{$regex:"tutorialspoint",$options:"$i"}})`  

该查询返回的一个结果文档中含有不同大小写的 “tutorialspoint” 文本。  

```  
{
"_id" : ObjectId("53493d37d852429c10000004"),
"post_text" : "hey! this is my post on TutorialsPoint", 
"tags" : [ "tutorialspoint" ]
} 


```  

## 使用正则表达式来处理数组元素  

我们还可以在数组字段上使用正则表达式。在实现标签的功能时，这尤为重要。假如想搜索标签以 “tutorial” 开始（tutorial、tutorials、tutorialpoint 或 tutorialphp）的帖子，可以使用下列代码：  

`>db.posts.find({tags:{$regex:"tutorial"}})`

## 优化正则表达式查询  

- 如果文档字段已经设置了索引，查询将使用索引值来匹配正则表达式，从而使查询效率相对于扫描整个集合的正则表达式而言大大提高。  
- 如果正则表达式为前缀表达式，所有的匹配结果都要在前面带有特殊的前缀字符串。比如，如果正则表达式为 **^tut**，那么查询将搜索所有以 **tut** 开始的字符串。  






