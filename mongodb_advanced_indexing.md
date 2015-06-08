
#  高级索引  


假如一个 **users** 集合中具有下列文档：  

```  
{
   "address": {
      "city": "Los Angeles",
      "state": "California",
      "pincode": "123"
   },
   "tags": [
      "music",
      "cricket",
      "blogs"
   ],
   "name": "Tom Benzamin"
}

```  

上述文档包含一个**地址子文档**（address sub-document）与一个**标签数组**（tags array）。  


## 索引数组字段  

假设我们想要根据标签来搜索用户文档。首先在集合中创建一个标签数组的索引。  

反过来说，在标签数组上创建一个索引，也就为每一个字段创建了单独的索引项。因此在该例中，当我们创建了标签数组的索引时，也就为它的music（音乐）、cricket（板球）以及 blog（博客）值创建了独立的索引。  

使用下列命令创建标签数据的索引：  

`>db.users.ensureIndex({"tags":1})`  

创建完该索引后，按照如下方式搜索集合中的标签字段：  

`>db.users.find({tags:"cricket"})`  

为了验证所使用索引的正确性，使用 **explain** 命令，如下所示：    

`>db.users.find({tags:"cricket"}).explain()`  

上述 explain 命令的执行结果是 "cursor" : "BtreeCursor tags_1"，表示使用了正确的索引。  

## 索引子文档字段  

假设需要根据市（city）、州（state）、个人身份号码（pincode）字段来搜索文档。因为所有这些字段都属于地址子文档字段的一部分，所以我们将在子文档的所有字段上创建索引。  

使用如下命令在子文档的所有三个字段上创建索引：  

`>db.users.ensureIndex({"address.city":1,"address.state":1,"address.pincode":1})`  

一旦创建了索引，就可以使用索引来搜索任何子文档字段：  

`>db.users.find({"address.city":"Los Angeles"})`  

记住，查询表达式必须遵循指定索引的顺序。因此上面创建的索引将支持如下查询：  

`>db.users.find({"address.city":"Los Angeles","address.state":"California"}) ` 

另外也支持如下这样的查询：  

`>db.users.find({"address.city":"LosAngeles","address.state":"California","address.pincode":"123"})`  



