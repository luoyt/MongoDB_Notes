---
noteId: "668c66905d2211ea8b3acbf1bbcc7fe6"
tags: []

---

### MongoDB简介
> MongoDB是一个开源、高性能、无模式的文档型数据库，当初的设计就是用于简化开发和方便扩展，是NoSQL数据库产品中的一种。是最像关系型数据库（MySQL）的非关系型数据库。
> 它支持的数据结构非常松散，是一种类似于JSON的格式叫BSON，所以它既可以存储比较复杂的数据类型，又相当的灵活。

> MongoDB中的记录是一个文档，它是一个由字段和值对（field：value）组成的数据结构。MongoDB文档类似于JSON对象，即一个文档认为就是一个对象。字段的数据类型是字符型，它的值除了使用基本的一些类型外，还可以包括其他文档、普通数据和文档数据。

### 体系结构
MySQL和MongoDB对比
- 组成
    关系型数据库
        1. 数据库
        2. 表
        3. 行
    MongoDB数据库
        1. 数据库
        2. Collection(集合)
        3. Document(文档)

SQL术语/概念|MongoDB术语/概念|解释/说明
--|:--:|--:
database|database|数据库
table|collection|数据库表/集合
row|document|数据记录行/文档
column|field|数据字段/域
index|index|索引
table joins||表连接，MongoDB不支持
|嵌入文档|MongoDB通过嵌入式文档来代替多表连接
primary key|primary key|主键，MongoDB自动将_id字段设置为主键

### 数据模型
> MongoDB的最小存储单位就是文档（document）对象。文档（document）对象对应于关系型数据库的行。数据在MongoDB中以BSON(Binary-JSON)文档的格式存储在硬盘上
> BSON（Binary Serialized Document Format）是一种类似JSON的一种二进制的存储格式，简称为Binary JSON。BSON和JSON一样，支持内嵌的文档对象和数据对象。但是BSON有JSON没有的一些数据类型，如Date和BinData类型

注意:对象id是文档的12字节的唯一ID

MongoDB的特点
MongoDB主要有一以下特点：
1. 高性能
2. 高可用
3. 高扩展
4. 丰富的查询支持
    > MongoDB支持丰富的茶余语言，支持读和写操作（CRUD），比如数据聚合、文本搜索和地理空间查询等。
5. 其他特点：如无模式（动态模式）、灵活地文档模型

### MongoDB安装
1. 下载（ZIP/EXE）
注意：MongoDB的命名：xyz;
y为奇数时表示当前版本为开发板，如：1.5.2、4.1.13
y为偶数时表示当前版本为稳定版，如：1.6.3、4.0.10；
z是版本修正号，数字越大越好。

2. 解压安装启动
将压缩包解压到一个目录中。
在解压目录中，手动建立一个目录用于存放数据文件，如data/db

方式:1：
命令行参数方式启动服务
在bin目录找那个打开命令行提示符，输入如下命令
```
mongod --dbpath=..\data\db

```
我们在启动信息可以看到、mongoDB的默认端口是27017，如果我们想改变默认的启动端口，可以通过--port来指定端口

为了方便我们每次启动，可以将安装目录的bin目录设置到环境变量中，bin目录下是一些常用命令，比如mongod是启动服务用的，mongo客户端是按揭服务用的。

方式2：
在解压目录中新建config文件夹，该文件夹中新建配置文件mongod.conf，如下
```
storge:
    #这是路径
    dbPath:
```
注意：
1. 配置文件中如果使用双引号，比如路径地址，自动会将双引号转义，如果不转义，则会报错：

解决:
a. \换成/或\\
b. 如果路径中没有空格，则无需加引号。

2. 配置文件中不能以Tab分割字段
解决：
将其转换为空格

启动方式：
```
mongod -f ../config/mongod.conf

```
或
```
mongod --config ../config/mongod.conf

```

### 连接数据库
1. Shell连接（mongo命令）
在命令·提示符输入以下shell命令即可完成登录
```
mongo/mongo --host=127.0.0.1 --port=27017

```

查看已有的数据库
```
show databases

```

退出mongoDB
```
exit

```

更多参数可以通过帮助查看：
mongo --help

提示：
MongoDB javascript shell 是一个基于JavaScript的计时器，故是支持js程序的



















    

