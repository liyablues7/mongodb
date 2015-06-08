MongoDB 并不支持**多文档原子事务**（multi-document atomic transactions）。但它提供了针对单个文档的原子操作。假如一个文档包含数百个字段，则 update 语句将更新所有的字段，或者一个也不更新，从而维持了文档级的原子性。  

## 原子操作数据模型  

维持原子性的建议方法是利用**内嵌文档**（embedded document）将所有经常更新的相关信息都保存在一个文档中。这能确保所有针对单一文档的更新具有原子性。  

考虑下列关于产品的文档：  

```  
{
"_id":1,
"product_name": "Samsung S3",
"category": "mobiles",
"product_total": 5,
"product_available": 3,
"product_bought_by": [
{
"customer": "john",
"date": "7-Jan-2014"
},
{
"customer": "mark",
"date": "8-Jan-2014"
}
]
}

```  

在这个文档中，将购买产品的顾客的信息内嵌在 **product_bought_by** 字段中。无论何时，只要新顾客购买产品，我们就能使用 **product_available** 字段查看产品是否还有足够的数量。如果产品还有，就减少 product_available 字段值，并且将新顾客的内嵌文档插入到 product_bought_by 字段中。使用 **findAndModify** 命令来实现该功能，因为它能同时搜索并更新文档。  

```  
>db.products.findAndModify({ 
query:{_id:2,product_available:{$gt:0}}, 
update:{ 
$inc:{product_available:-1}, 
$push:{product_bought_by:{customer:"rob",date:"9-Jan-2014"}} 
}    
})

```  

上述内嵌文档并使用 findAndModify 查询的方法确保了，只有当产品还有足够数量时，才更新产品购买信息。在整个过程中，同一查询中的事务是原子性的。  

与之相反的情况是，我们可能会想分别保持产品可用性与购买产品的顾客信息。在这种情况下，我们会首先使用第一个查询来检查产品是否够用。然后在第二个查询中更新购买信息。但在这两个查询的执行过程之间，其他一些顾客也可能会购买了产品，从而使产品变得不够用了。由于没有了解到这种情况，我们的第二个查询根据第一个查询的结果进行了更新。这将造成数据库的不一致性。   




