# 使用 c 语言在 MongoDB 上进行 CRUD 操作。

> 原文：<https://blog.devgenius.io/crud-operations-on-mongodb-using-c-4d90b3510dfc?source=collection_archive---------6----------------------->

![](img/83aef3bb03a80de20da278ecb369e378.png)

托拜厄斯·菲舍尔在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

**什么是 MongoDB？**

MongoDB 是一个面向文档的 NoSQL 数据库，用于大容量数据存储。MongoDB 不像传统的关系数据库那样使用表和行，而是使用集合和文档。文档由键值对组成，键值对是 MongoDB 中的基本数据单元。集合包含一组文档和函数，相当于关系数据库表。MongoDB 是一个在 2000 年代中期问世的数据库。

**先决条件:**

操作系统:UBUNTU 16.04 及以上版本。

MongoDB 社区服务器& MongoDB Compass

c 代码编译器——你必须为 c 代码安装任何编译器。

## **构建并安装 MongoDB C 驱动**

这些是构建 MongoDB C 驱动程序的命令。

```
export SOURCE_ROOT=/<source_root>/
sudo apt-get update
sudo apt-get install -y cmake gcc libsasl2-dev libssl-dev make pkg-config tar wget
cd $SOURCE_ROOT
wget https://github.com/mongodb/mongo-c-driver/releases/download/1.16.2/mongo-c-driver-1.16.2.tar.gz
tar -xzf mongo-c-driver-1.16.2.tar.gz
cd mongo-c-driver-1.16.2
mkdir cmake-build
cd cmake-build
cmake -DENABLE_AUTOMATIC_INIT_AND_CLEANUP=OFF ..
make
sudo make install
```

设置环境变量:

```
export LD_LIBRARY_PATH=/usr/local/lib
export PKG_CONFIG_PATH=/usr/local/lib/pkgconfig
```

下面的示例用于执行一个基本测试，以确保 MongoDB C 驱动程序按预期工作，将下面的示例保存在一个名为 insert.c 的文件夹中

**insert.c**

```
#include<mongoc.h>#include<bson.h>#include<stdio.h>int main(){mongoc_client_t *client;mongoc_collection_t *collection;mongoc_cursor_t *cursor;bson_error_t error;bson_t *doc;mongoc_init();client = mongoc_client_new("mongodb://localhost:27017");collection = mongoc_client_get_collection(client,"mydb","students");doc=bson_new();BSON_APPEND_UTF8(doc,"_id","1098");BSON_APPEND_UTF8(doc,"firstName","SIDDHANT");BSON_APPEND_UTF8(doc,"lastName","JOHARI");if(!mongoc_collection_insert_one(collection,doc,NULL,NULL,&error)) printf("%s\n",error.message);bson_destroy(doc);mongoc_collection_destroy(collection);mongoc_client_destroy(client);mongoc_cleanup();printf("Done");return 0;}
```

我将分几步解释这个例子。

**第一步。与 MongoDB 社区服务器连接。**

第一步，我们必须建立与 MongoDB 社区服务器的连接。

```
*mongoc_client_t *client=mongoc_client_new("mongodb://localhost:27017");*
```

**第二步。访问/创建集合。**

在 MongoDB 中，如果数据库/集合已经存在，那么它返回对数据库的引用。它将自动创建一个新的数据库/集合，然后返回对新数据库/集合的引用。

```
mongoc_collection_t *collection = mongoc_client_get_collection(client,"mydb","students");
```

**第三步。正在创建文档。**

这里的文档指的是一个结构体，它包含了所有关于要插入的数据的信息。

```
bson_t *doc=bson_new();
BSON_APPEND_UTF8(doc,"_id","1098");
BSON_APPEND_UTF8(doc,"firstName","SIDDHANT");
BSON_APPEND_UTF8(doc,"lastName","JOHARI");
```

**第四步。将文档插入集合。**

创建文档后，我们将使用 mongoc_collection_insert_one 方法将其插入到集合中。

```
if(!mongoc_collection_insert_one(collection,doc,NULL,NULL,&error))
{
printf("%s\n",error.message);
}
```

**步骤五。为所有引用释放内存。**

```
bson_destroy(doc);
mongoc_collection_destroy(collection);
mongoc_client_destroy(client);
mongoc_cleanup();
```

编译并执行:

```
gcc insert.c $(pkg-config --libs --cflags libmongoc-1.0) -o insert.exe
./insert.exe
```

输出:

```
Done
```

如果您已经正确地完成了所有事情，那么现在，您就可以执行其他操作了，比如删除、更新。

**update.c**

```
#include<mongoc.h>
#include<bson.h>
#include<stdio.h>
int main()
{
mongoc_client_t *client;
mongoc_collection_t *collection;
mongoc_cursor_t *cursor;
bson_error_t error;
bson_t *doc, *query;
mongoc_init();
client = mongoc_client_new("mongodb://localhost:27017");
collection = mongoc_client_get_collection(client,"mydb","students");
query=bson_new();
BSON_APPEND_UTF8(query,"_id","1098");
doc = BCON_NEW ("$set", "{", "_id",BCON_UTF8("1098"), "firstName",BCON_UTF8("Elon"),"lastName",BCON_UTF8("Musk"), "}");
if(!mongoc_collection_update_one(collection,query,doc,NULL,NULL,&error)) printf("%s\n",error.message);
bson_destroy(query);
bson_destroy(doc);
mongoc_collection_destroy(collection);
mongoc_client_destroy(client);
mongoc_cleanup();
printf("Done");
return 0;
}
```

在本例中，我们将更新 id 为“1098”的文档的名字和姓氏。

对于本例，我们将按照 Insert 的步骤 1、步骤 2 和步骤 5 进行。

**第三步。创建文档。**

现在我们将创建两个文档，一个用于搜索以前的数据，另一个用于替换旧的数据。

```
bson_t *query=bson_new();
BSON_APPEND_UTF8(query,"_id","1098");bson_t *doc = BCON_NEW ("$set", "{", "_id",BCON_UTF8("1098"), "firstName",BCON_UTF8("Elon"),"lastName",BCON_UTF8("Musk"), "}");
```

**第四步。正在更新集合中的文档。**

现在创建了文档查询和文档之后，我们将使用 mongoc_collection_update_one 方法更新集合。

```
if(!mongoc_collection_update_one(collection,query,doc,NULL,NULL,&error)) printf("%s\n",error.message);
```

**delete.c**

```
#include<mongoc.h>
#include<bson.h>
#include<stdio.h>
int main()
{
mongoc_client_t *client;
mongoc_collection_t *collection;
mongoc_cursor_t *cursor;
bson_error_t error;
bson_t *query;
mongoc_init();
client = mongoc_client_new("mongodb://localhost:27017");
collection = mongoc_client_get_collection(client,"mydb","students");
query=bson_new();
BSON_APPEND_UTF8(query,"_id","1098");
if(!mongoc_collection_delete_one(collection,query,NULL,NULL,&error)) printf("%s\n",error.message);
bson_destroy(query);
mongoc_collection_destroy(collection);
mongoc_client_destroy(client);
mongoc_cleanup();
printf("Done");
return 0;
}
```

在本例中，我们将删除 id 为“1098”的文档。

为此，我们将遵循插入示例中的步骤 1、步骤 2 和步骤 5。

**第三步。创建查询文档。**

在本例中，我们将创建一个查询文档，该文档引用将要从集合中删除的数据。

```
bson_t *query=bson_new();
BSON_APPEND_UTF8(query,"_id","1098");
```

**第四步。从集合中删除文档。**

现在创建查询文档后，我们将使用 mongoc_collection_delete_one 方法从集合中删除文档。

```
if(!mongoc_collection_delete_one(collection,query,NULL,NULL,&error)) printf("%s\n",error.message);
```

[**结论**](https://medium.com/@johari.sjohari/crud-operations-on-mongodb-using-c-4d90b3510dfc) **:**

在这篇博客中，我们学习了如何使用 c 在 MongoDB 上执行 CRUD 操作。

谢谢你，

悉达多·乔哈里