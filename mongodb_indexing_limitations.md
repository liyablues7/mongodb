## 额外开销  

每个索引都会占据一些空间，从而也会在每次插入、更新与删除操作时产生一定的开销。所以如果集合很少使用读取操作，就尽量不要使用索引。  

## 内存使用  

因为索引存储在内存中，所以应保证索引总体的大小不超过内存的容量。如果索引总体积超出了内存容量，就会删除部分索引，从而降低性能。  

## 查询限制  

当查询使用以下元素时，不能使用索引：  

- 正则表达式或否定运算符（`$nin`、`$not`，等等）  
- 算术运算符（比如 `$mod`）  
- **$where** 子句

因此，经常检查查询使用的索引是一个明智的做法。  

## 索引键限制  

自 MongoDB 2.6 版本起，如果已有索引字段的值超出了索引键限制，则无法创建索引。  

## 插入文档超过索引键限制

如果文档的索引字段值超出了索引键的限制，MongoDB 不会将任何文档插入已索引集合。类似于使用 mongorestore 和 mongoimport 工具时的情况。   


## 最大范围  

- 集合索引数不能超过 64 个。  
- 索引名称长度不能大于 125 个字符。  
- 复合索引最多能有 31 个被索引的字段。  
