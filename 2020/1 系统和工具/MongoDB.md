# MongoDB

> 由C++ 编写的非关系型数据库， 比较像关系型数据库。 使用JSON存储数据。

mongo名词和传统关系型数据库对应：

| 关系型数据库 | 非关系型数据库 | 解释                      |
| ------------ | -------------- | ------------------------- |
| database     | database       | 数据库                    |
| table        | collection     | 数据库表，NoSql中为集合   |
| row          | document       | 数据库行，保存一条记录    |
| column       | field          | 数据库列，NoSql中为属性。 |
| index        | index          | 索引                      |
| join         | 无             | 表连接，mongo不支持。     |
| primary key  | primary key    | 主键                      |

## 数据库

一个mongodb中可以建立多个数据库。

MongoDB的默认数据库为"db"，该数据库存储在data目录中。

MongoDB的单个实例可以容纳多个独立的数据库，每一个都有自己的集合和权限，不同的数据库也放置在不同的文件中。

**"show dbs"** 命令可以显示所有数据的列表。

数据库也通过名字来标识。数据库名可以是满足以下条件的任意UTF-8字符串。

- 不能是空字符串（"")。
- 不得含有' '（空格)、.、$、/、\和\0 (空字符)。
- 应全部小写。
- 最多64字节。

有一些数据库名是保留的，可以直接访问这些有特殊作用的数据库。

- **admin**： 从权限的角度来看，这是"root"数据库。要是将一个用户添加到这个数据库，这个用户自动继承所有数据库的权限。一些特定的服务器端命令也只能从这个数据库运行，比如列出所有的数据库或者关闭服务器。
- **local:** 这个数据永远不会被复制，可以用来存储限于本地单台服务器的任意集合
- **config**: 当Mongo用于分片设置时，config数据库在内部使用，用于保存分片的相关信息。

## 文档(Document)

文档是一组键值(key-value)对(即 BSON)。MongoDB 的文档不需要设置相同的字段，并且相同的字段不需要相同的数据类型，这与关系型数据库有很大的区别，也是 MongoDB 非常突出的特点。

一个简单的文档例子如下：

```json
{"site":"www.runoob.com", "name":"菜鸟教程"}
```

> 需要注意的是：
>
> 1. 文档中的键/值对是有序的。
> 2. 文档中的值不仅可以是在双引号里面的字符串，还可以是其他几种数据类型（甚至可以是整个嵌入的文档)。
> 3. MongoDB区分类型和大小写。
> 4. MongoDB的文档不能有重复的键。
> 5. 文档的键是字符串。除了少数例外情况，键可以使用任意UTF-8字符。
>
> 文档键命名规范：
>
> - 键不能含有\0 (空字符)。这个字符用来表示键的结尾。
> - .和$有特别的意义，只有在特定环境下才能使用。
> - 以下划线"_"开头的键是保留的(不是严格要求的)。



## 集合

集合就是 MongoDB 文档组，类似于 RDBMS （关系数据库管理系统：Relational Database Management System)中的表格。

集合存在于数据库中，集合没有固定的结构，这意味着你在对集合可以插入不同格式和类型的数据，但通常情况下我们插入集合的数据都会有一定的关联性。



当第一个文档插入时，集合就会被创建。

### 合法的集合名

- 集合名不能是空字符串""。
- 集合名不能含有\0字符（空字符)，这个字符表示集合名的结尾。
- 集合名不能以"system."开头，这是为系统集合保留的前缀。
- 用户创建的集合名字不能含有保留字符。有些驱动程序的确支持在集合名里面包含，这是因为某些系统生成的集合中包含该字符。除非你要访问这种系统创建的集合，否则千万不要在名字里出现$。　



# 安装

在docker中安装：

```shell
# 拉取默认最新版的镜像
docker pull mongo
# 默认端口 27017
docker run -itd -p 27017:27017 --name test mongo --auth
# 启动管理工具,添加用户和密码
docker exec -it test mongo admin 

# 创建一个名为 admin，密码为 123456 的用户。
>  db.createUser({ user:'yudie',pwd:'123456',roles:[ { role:'root', db: 'admin'}]});
# 尝试使用上面创建的用户信息进行连接。
> db.auth('yudie', '123456')
```

> 附：
>
> 1.数据库用户角色：read、readWrite;
> 2.数据库管理角色：dbAdmin、dbOwner、userAdmin；
> 3.集群管理角色：clusterAdmin、clusterManager、clusterMonitor、hostManager；
> 4.备份恢复角色：backup、restore
> 5.所有数据库角色：readAnyDatabase、readWriteAnyDatabase、userAdminAnyDatabase、dbAdminAnyDatabase
> 6.超级用户角色：root



# 连接

连接字符串：

```java
mongodb://username:password@host:port/databaseName
```

也可以使用多主机， 一次连接多个服务器：

```java
mongodb://[username:password@]host[:port][, host2:port2][,hostN:portN]/databaseName?[options]
```

**可选项：**

参考如下链接， 感觉用不到。

[mongoDB 连接](https://www.runoob.com/mongodb/mongodb-connections.html)



java 程序使用mongoDB驱动连接：

```xml
<dependency>
    <groupId>org.mongodb</groupId>
    <artifactId>mongodb-java-driver</artifactId>
    <version>3.4.3</version>
</dependency>
```

```java
@Test
public void testConnection(){
    // 直接连接
    MongoClient client = new MongoClient("host", port);
    // 或者使用连接字符串
    MongoClientURI uri = new MongoClientURI("mongodb://user:password@host:port");
    MongoClient client = new MongoClient(uri);
    
    // 连接数据库
    MongoDatabase database = client.getDatabase("name");
    MongoCollection coll = database.getCollection("tableName");
    Document doc = coll.find().first();
    System.out.println(doc.toJSON());
}
```

# 使用

```shell
# 查看所有数据库
show dbs
# 切换到指定数据库，如果不存在则创建. 但是只有数据库中至少有一个有效collection时才会显示。
use DATABASE_NAME

# 查看当前使用的数据库
db

# 删除当前使用的数据库
db.dropDatabase()
```

## 数据库操作

连接建立时， 如果没有指定数据库默认使用`test` 数据库。

### 创建数据库

使用`use <database_name>` 切换到指定集合， 如果目标集合不存在则会创建。

> **注意:** 
>
> 在 MongoDB 中，集合只有在内容插入后才会创建! 就是说，创建集合(数据表)后要再插入一个文档(记录)，集合才会真正创建。

### 删除数据库

```js
db.dropDatabase();
```

删除当前使用的数据库。即`db` 所输出的数据库名。 



### 查看数据库

```shell
# 查看所有数据库列表。
show dbs;
# 查看当前数据库。
db;
```







## 集合操作：collection

### 创建集合

```js
db.createCollection(<name>, [option]);
```

- name : 需要创建的集合名称。
- option： 指定的可选项。

options 可以是如下参数：

| 字段        | 类型 | 描述                                                         |
| :---------- | :--- | :----------------------------------------------------------- |
| capped      | 布尔 | （可选）如果为 true，则创建固定集合。固定集合是指有着固定大小的集合，当达到最大值时，它会自动覆盖最早的文档。 **当该值为 true 时，必须指定 size 参数。** |
| autoIndexId | 布尔 | （可选）如为 true，自动在 _id 字段创建索引。默认为 false。   |
| size        | 数值 | （可选）为固定集合指定一个最大值，以千字节计（KB）。 **如果 capped 为 true，也需要指定该字段。** |
| max         | 数值 | （可选）指定固定集合中包含文档的最大数量。                   |

在插入文档时，MongoDB 首先检查固定集合的 size 字段，然后检查 max 字段。

> `capped collection` 固定大小集合，是指使用插入顺序存放的固定大小的集合。 拥有更改的性能。
>
> capped collection 的大小和记录的大小一旦指定不能修改。（即文档更新后不能超过原来大小， 否则会失败）



例子：

```js
# 创建集合，如果没有使用use指定数据库，默认存放在test库中。
db.createCollection("student");
```



### 删除集合

```js
# 删除collection——name 指定的collection
db.<collection_name>.drop();
```



### 查看当前数据库所有集合

```js
# 相比下面的更准确。
show collections
show tables
```



## 文档操作： document

### 插入文档

```js
db.<collection_name>.insert(<JSON>);
```

如果此时使用的`collection_name` 不存在，mongo会 自动创建集合。

JSON内容可以是 `key-value` 也可以是任意 `key-object(JSON)`

#### 批量插入

```js
# 插入一条
db.collection.insertOne()
# 插入多条记录（文档） 类似java 的可变参数。
db.collection.insertMany()
```

使用数组批量插入， 把需要插入的对象放进数组中，然后  `insert`

```js
var arr = [];

for (var i = 0; i < 10; i ++){
    arr.push({key: "value"})
}

db.<coll>.insert(arr);
```



**例子：**

```js
db.table.insert({
    name: "zhangsan",
    age: 18,
    type: "",
    ...
});
```



### 更新文档

```js
db.collection.update(
   <query>,
   <update>,
   [{
     upsert: <boolean>,
     multi: <boolean>,
     writeConcern: <document>
   }]
)
```

- query: 用于匹配查询的JSON字符串。
- update： 期望更新后的文档。
- upsert：如果query无匹配结果，是否直接插入。 默认为false
- multi: 默认为false， 只返回一条记录。 true为返回多条。
- writeConcern： 异常级别。



remove() 方法 并不会真正释放空间。

需要继续执行 db.repairDatabase() 来回收磁盘空间。

```js
db.repairDatabase()
// 或者
db.runCommand({ repairDatabase: 1 })
```

### 修改器

`$set` `$unset` 可以与在更新操作中使用， 更新/创建/删除 指定的键值。

`$push` 对值为数组的键 添加创建元素

### 删除文档

#### ~~历史版本~~

语法：

```js
// 基础语法。
db.<collection_name>.remove(<query>, <just_one>);

// 2.6 之后的版本
db.collection.remove(
   <query>,
   {
     justOne: <boolean>,
     writeConcern: <document>
   }
)
```

参数：

| name         | value                   | 解释                                                        |
| ------------ | ----------------------- | ----------------------------------------------------------- |
| [query]      | JSON                    | 匹配条件                                                    |
| [just_one]   | true \| 1   默认为false | 设为ture则只删除一条记录，false删除所有[query] 匹配的记录。 |
| writeConcern |                         | 抛出的异常级别                                              |

#### 全新版本

语法：

```js
# 删除多个
db.<collection_name>.deleteMany();
# 删除一个
db.<collection_name>.deleteOne();
```



### 查询文档

语法：

```js
# 查询
db.<collection_name>.find([query], [projection]);

db.<collection_name>.findOne([query], [projection]);
```

- **query** ：可选，使用查询操作符指定查询条件
- **projection** ：可选，使用投影操作符指定返回的键。查询时返回文档中所有键值， 只需省略该参数即可（默认省略）。

如果你需要以易读的方式来读取数据，可以使用 pretty() 方法，语法格式如下：

```js
db.<collection>.find().pretty()
```

pretty() 方法以格式化的方式来显示所有文档。











# 管理

## 用户管理

### 创建用户

```js
# 语法格式
db.createUser({
    user: <用户民>,
    pwd: <密码>,
    customData: { <any info> },
    roles: [
        {role: <角色名>, db: <所属数据库>} | <角色名>,
    	...
    ]
});
```



### 删除用户

```js
db.dropUser(<userName>);
```

