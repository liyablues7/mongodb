Rockmongo 是一个 MongoDB 的管理工具，可以用来管理服务器、数据库、集合、文档、索引以及很多其他内容。它的操作简单便利，易于读写创建文档。它的作用有点像是 PHP 和 MySQL 所使用的 PHPMyAdmin 工具。  

## 下载 Rockmongo  

从[这里](http://rockmongo.com/downloads)下载最新版的 Rockmongo。  

## 安装 Rockmongo  

下载完毕后，可以将包解压缩至服务器的根目录处，将解压文件夹重新命名为 **rockmongo**。打开任何一个浏览器，从rockmongo 文件夹处访问 **index.php** 页面。当询问用户名和密码时，分别输入 admin/admin。  

## 使用 Rockmongo  

下面介绍一些 Rockmongo 的基本操作。  

- **创建新数据库**   

要想创建新数据库，请点击 **Databases** 标签，然后点击 **Create New Database**。在下一个界面中，为新数据库命名，然后点击 **Create** 即可。你就会看到一个新数据库添加到了左边的面板中。  

- **创建新集合**  

要想在数据库中创建新的集合，从左边的面板中选择数据库，点击最上方的 **New Collection**。然后提供新集合的名称，不用理会其他字段（Is Capped，Size and Max），点击 **Create** 即可。新的集合就创建好了，就显示在左边的面板中。  

- **创建新文档**  

要想创建新文档，点击想要在其中添加文档的集合。点击一个集合，就能看到其中的所有文档。点击最上方的 **Insert** 创建新文档。输入的文档数据既可以是 JSON 格式，也可以是数组格式。输入文档数据之后，点击 **Save** 即可创建好一个新的文档。  

- **数据的导出与导入**  

要想导入/导出任何集合中的数据，点击该集合，然后点击上方面板的 **Export/Import**。根据接下来的指令，将数据导出为 zip 格式的压缩数据，或者将同样格式的 zip 文件导入。
