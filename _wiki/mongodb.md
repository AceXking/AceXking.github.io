---
layout: wiki
title: mongodb
categories: mongodb
description: mongodb的一些常用命令和用法。
keywords: mongodb
---
# 1.mongodb

MongoDB 是由C++语言编写的，是一个基于分布式文件存储的开源数据库系统。 

MongoDB 将数据存储为一个文档，数据结构由键值(key=>value)对组成。 

# 2.安装mongodb

1.导入包管理系统使用的公钥 

```
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 0C49F3730359A14518585931BC711F9BA15703C6
```

 2.为MongoDB创建一个列表文件 

```
echo "deb [ arch=amd64,arm64 ] http://repo.mongodb.org/apt/ubuntu xenial/mongodb-org/3.4 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-3.4.list
```

3.更新本地包数据库 

```
sudo apt-get update

```

4.安装最新版本的MongoDB 

```
sudo apt-get install -y mongodb-org
```

5.查看配置文件

配置文件mongod.conf所在路径:

/etc/mongod.conf

```
# mongod.conf

# for documentation of all options, see:
#   http://docs.mongodb.org/manual/reference/configuration-options/

# Where and how to store data.
storage:
  dbPath: /var/lib/mongodb   #数据库存储路径
  journal:
    enabled: true
#  engine:
#  mmapv1:
#  wiredTiger:


# where to write logging data.
systemLog:
  destination: file
  logAppend: true     #以追加的方式写入日志
  path: /var/log/mongodb/mongod.log   #日志文件路径

# network interfaces
net:
  port: 27017
  bindIp: 127.0.0.1   #绑定监听的ip 127.0.0.1只能监听本地的连接，可以改为0.0.0.0


#processManagement:

#security:

#operationProfiling:

#replication:

#sharding:

## Enterprise-Only Options:

#auditLog:

#snmp:
```

6.启动和关闭MongoDB

```
sudo service mongod start  # 启动
sudo service mongod stop   # 关闭


hupeng@hupeng-vm:~$ ps aux | grep mongod   # 查看守护进程mongod的运行状态
mongodb   18454  9.5  1.5 292152 61952 ?        Ssl  12:27   0:00 /usr/bin/mongod --quiet --config /etc/mongod.conf
hupeng    18475  0.0  0.0  15964   936 pts/4    R+   12:27   0:00 grep --color=auto mongod
```

卸载

1.关闭守护进程mongod

```
sudo service mongod stop
```

2.卸载安装的软件包

```
sudo apt-get purge mongodb-org*
```

3.移除数据库和日志文件（数据库和日志文件的路径取决于/etc/mongod.conf文件中的配置)

```
sudo rm -r /var/log/
mongodb
sudo rm 
-r /var/lib/mongodb
```

# 3.mongodb概念解析

在mongodb中基本的概念是文档、集合、数据库，下面我们挨个介绍。

| SQL术语/概念 | MongoDB术语/概念 | 解释/说明                           |
| ------------ | ---------------- | ----------------------------------- |
| database     | database         | 数据库                              |
| table        | collection       | 数据库表/集合                       |
| row          | document         | 数据记录行/文档                     |
| column       | field            | 数据字段/域                         |
| index        | index            | 索引                                |
| table joins  |                  | 表连接,MongoDB不支持                |
| primary key  | primary key      | 主键,MongoDB自动将_id字段设置为主键 |

**文档**

文档是一组键值(key-value)对(即BSON)。MongoDB 的文档不需要设置相同的字段，并且相同的字段不需要相同的数据类型，这与关系型数据库有很大的区别，也是 MongoDB 非常突出的特点。

一个简单的文档例子如下：

```
{"site":"www.runoob.com", "name":"菜鸟教程"}
```

需要注意的是：

1. 文档中的键/值对是有序的。
2. 文档中的值不仅可以是在双引号里面的字符串，还可以是其他几种数据类型（甚至可以是整个嵌入的文档)。
3. MongoDB区分类型和大小写。
4. MongoDB的文档不能有重复的键。
5. 文档的键是字符串。除了少数例外情况，键可以使用任意UTF-8字符。

文档键命名规范：

- 键不能含有\0 (空字符)。这个字符用来表示键的结尾。
- .和$有特别的意义，只有在特定环境下才能使用。
- 以下划线"_"开头的键是保留的(不是严格要求的)。

**集合**

集合就是MongoDB文档组，类似于MySQL的表格

# 4.mongdb命令

**增删改查条件都要加双引号，只有数字类型不用加**

一个mongodb中可以建立多个数据库，mongodb的默认数据库为db，该数据存储在data的目录中，

MongoDB的单个实例可以容纳多个独立的数据库，每一个都有自己的集合和权限，不同的数据库也放置在不同的文件中。 

​                                                            MongoDB 创建数据库

```
mongo           进入数据库

show dbs       查看所有库

db                    查看当前所在库

use  库名        创建或者选择库，如果数据库不存在，则创建数据库，否则切换到指定数据库。 

创建的数据库 并不在数据库的列表中， 要显示它，我们需要向  数据库插入一些数据。 

db.user.find()     查看集合中的所有文档  查看表中所有数据

show collections    查看当前库中的所有集合,所有表
show tables  

db.dropDatabase()    删除当前数据库

db.集合名.drop()          删除集合
```

## 4.1.mongodb插入文档

添加数据      使用 insert() 或 save() 方法向集合中插入文档 

使用 insert 添加  

```
格式： db.集合名.insert()
db.user.insert({"name":'zhangsan','age':20,'sex':'男'})
```

```
添加数据,指定_id
db.user.insert({"_id":10010,"name":'lisi','age':20,'sex':'男'})
```

save() ，如果在执行时不指定 _id 则添加数据，如果指定 _id 字段，则会更新该 _id 的数据。 

```
 如果不指定_id,则添加
db.user.save({'name':'aabbcc','age':22,'email':'zbc@qq.com'})

-- 如果指定id会去寻找 _id的数据,然后替换
db.user.save({'_id':ObjectId("5af2481389c88795375d5762"),'name':'田六','age':22,'email':'zbc@qq.com'})
```

批量添加：

```
可以定义变量,添加数据

a = {"name":'王五','age':22,'sex':'男'}

db.集合名.insert(a)


b = [
    {"name":'张三','age':23,'sex':'男'},
    {"name":'李四','age':24,'sex':'女'},
    {"name":'赵柳','age':25,'sex':'男'},
    {"name":'田七','age':26,'sex':'女'}
]

db.集合名.insert(b)
```

## 4.2.mongodb 修改数据

使用**update()**和**save()**方法来更新集合中的文档。 

update() 方法用于更新已存在的文档。语法格式如下：

```
db.集合名.update(
   <query>,
   <update>,
   {
     upsert: false,
     multi: true
   }
)
```

**参数说明：**

- **query** : update的查询条件，类似sql update查询内where后面的。
- **update** : update的对象和一些更新的操作符（如$,$inc...）等，也可以理解为sql update查询内set后面的
- **upsert** : 可选，这个参数的意思是，如果不存在update的记录，是否插入objNew,true为插入，默认是false，不插入。
- **multi** : 可选，mongodb 默认是false,只更新找到的第一条记录，如果这个参数为true,就把按条件查出来多条记录全部更新。

**实例**

```
-- 找到 name=田七 的数据,更新age字段,只更新找到的第一条数据,如果没有找到也不添加
db.集合名.update({'name':'田七'},{$set:{'age':29}})

-- multi默认为false,(只更新找到的第一条数据)
-- 找到name=田七的 数据,更新eamil字段,更新所有符合条件的数据,如果没有找到,也不添加
db.集合名.update({"name":"田七"},{$set:{'age':26,'email':'tq@qq.com'}},{multi:true})

-- upsert 默认为false,(如果找到不到符合条件的数据,也不添加)
-- 找到name=田七的 数据,更新eamil字段,更新所有符合条件的数据,如果没有找到,就添加数据
db.集合名.update({"name":"田八"},{$set:{'age':26,'email':'tq@qq.com'}},{multi:true,upsert:true})
```



**save() 方法会去寻找id数据，如果有则修改，如果没有则添加，用法跟添加save（）一样**

## 4.3.mongodb 删除数据

### 语法

remove() 方法的基本语法格式如下所示：

```
db.集合名.remove(
   <query>,
   <justOne>
)
```

如果你的 MongoDB 是 2.6 版本以后的，语法格式如下：

```
db.集合名.remove(
   <query>,
   {
     justOne: <boolean>,
     writeConcern: <document>
   }
)
```

**参数说明：**

- **query** :删除的文档的条件。
- **justOne** : （可选）如果设为 true 或 1，则只删除一个文档。
- **writeConcern** :（可选）抛出异常的级别。

```
-- 删除操作

-- 删除所有 age = 25 的数据

db.集合名.remove({'age':25})

-- 删除第一个 age=25

db.集合名.remove({'age':25},1)

-- 删除所有

db.集合名.remove({})

```

## 4.4mongod 查询文档

查询文档使用 find() 方法 ，查询条件里面写  '_id':0 是查出来的数据不要id

**语法**

MongoDB 查询数据的语法格式如下：

```
db.集合名.find(query, projection)
```

- **query** ：可选，指定查询条件
- **projection** ：可选，使用投影操作符指定返回的键。查询时返回文档中所有键值， 只需省略该参数即可（默认省略）。

如果你需要以易读的方式来读取数据，可以使用 pretty() 方法，语法格式如下：

```
>db.集合名.find().pretty()
```

pretty() 方法以格式化的方式来显示所有文档。



**MongoDB 与 RDBMS Where 语句比较**

如果你熟悉常规的 SQL 数据，通过下表可以更好的理解 MongoDB 的条件语句查询：

| 操作       | 格式                     | 范例                                        | RDBMS中的类似语句       |
| ---------- | ------------------------ | ------------------------------------------- | ----------------------- |
| 等于       | `{<key>:<value>`}        | `db.col.find({"by":"菜鸟教程"}).pretty()`   | `where by = '菜鸟教程'` |
| 小于       | `{<key>:{$lt:<value>}}`  | `db.col.find({"likes":{$lt:50}}).pretty()`  | `where likes < 50`      |
| 小于或等于 | `{<key>:{$lte:<value>}}` | `db.col.find({"likes":{$lte:50}}).pretty()` | `where likes <= 50`     |
| 大于       | `{<key>:{$gt:<value>}}`  | `db.col.find({"likes":{$gt:50}}).pretty()`  | `where likes > 50`      |
| 大于或等于 | `{<key>:{$gte:<value>}}` | `db.col.find({"likes":{$gte:50}}).pretty()` | `where likes >= 50`     |
| 不等于     | `{<key>:{$ne:<value>}}`  | `db.col.find({"likes":{$ne:50}}).pretty()`  | `where likes != 50`     |

**实例**

```
 -- 查询 age=25

db.集合名.find({'age':25})

-- 查询 age<25

db.集合名.find({'age':{$lt:25}})

-- 查询不等于

db.集合名.find({'age':{$ne:25}})
```



### 4.4.1mongodb and 和 or  条件

**mongodb and 条件**

**find() 方法可以传入多个键（key）,每个键（key）以逗号隔开**

```
db.集合名.find({key1:value1,key2:value2}).pretty()
```



**mongodb or 条件**

mongodb or 条件语句使用了关键字**$or**,语法格式如下：

```
db.集合名.find({$or:[{key1:value1}，{key2:value2}]})
```



**and 和 or 联合使用**

```
 查询 年龄>25 并且 name=a or name=b
 MySQL写法：
        select * from user where age >= 25 and (name='b' or name='a')
 mongodb写法：
        db.user.find({'age':{$gte:25},$or:[{'name':'a'},{'name':'b'}]})
```

## 4.5mongodb limit() 方法

limit()方法接受一个数字参数，该参数指定读取的几条数据。 

```
 limit(num) 限制查询条数
-- db.集合名.find().limit(1)
```

## 4.6mongodb skip() 方法

skip()方法来跳过指定数量的数据 

```
db.user.find({},{'name':1,'_id':0}).limit(1).skip(1)
```

## 4.7mongodb   排序 sort（） 方法

sort({key:1})  指定key作为排序字段,1为升序  -1为降序

```
db.集合名.find().sort({KEY:1})
```

## 4.8MongoDB 统计 count()方法

```
db.集合名.count()
```

## 4.9MongoDB 分组aggregate() 方法

aggregate下的 group分组

```
db.user.aggregate([{$group:{'_id':'$classid','total':{$sum:1}}}])
```

下表展示了一些聚合的表达式:

| 表达式 | 描述                               | 实例                                                         |
| ------ | ---------------------------------- | ------------------------------------------------------------ |
| $sum   | 计算总和。                         | db.mycol.aggregate([{$group : {_id : "$by_user", num_tutorial : {$sum : "$likes"}}}]) |
| $avg   | 计算平均值                         | db.mycol.aggregate([{$group : {_id : "$by_user", num_tutorial : {$avg : "$likes"}}}]) |
| $min   | 获取集合中所有文档对应值得最小值。 | db.mycol.aggregate([{$group : {_id : "$by_user", num_tutorial : {$min : "$likes"}}}]) |
| $max   | 获取集合中所有文档对应值得最大值。 | db.mycol.aggregate([{$group : {_id : "$by_user", num_tutorial : {$max : "$likes"}}}]) |