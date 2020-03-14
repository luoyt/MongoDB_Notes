---
noteId: "1c828c705d6111ea842439931df45c97"
tags: []

---

### 文档的CRUD
文档（document）的数据结构和JSON基本一样。
所有存储在集合中的数据都是BSON格式。

#### 文档的插入
1. 单个文档插入
使用insert()或save()方法向集合中插入文档，语法如下：
```
db.collection.insert(
    <document or array of documents>
    {
        writeConcern:<document>,
        ordered:<boolean>

    }
)

```
参数
Parameter|Type|Description
:--|:--:|--:
document|document or array|要插入到集合中的文档或者文档数组
writeConcern|document|
ordered|boolean|可选。如果为真，则按顺序插入数组中的文档，如果其中一个文档出现错误，MongoDB将返回而不处理数组中的其余文档。如果为假，则执行无序插入，如果其中一个文档出现错误，则继续处理数组中的文档。在版本2.6+中默认为TRUE

实例：要向comment的集合（表）中插入一条测试数据
```
db.commit.insert({"articleid":"100000", "content":"今日天气好，阳光明媚！", "userid":"1001", "nickname":"Rose", "createdatatime":new Date(), "likenum":"NumberInt(10)", "state":null})
```
提示：
1. comment集合中如果不存在，则会隐式创建
2. mongo中的数字，默认情况下是double类型，如果要存整型，必须使用函数NumberInt(整形数字)，否则取出来就有问题了。
3. 插入当前日期使用new Date()
4. 插入的数据没有指定_id,会自动生成主键值
5. 如果某字段没值，可以赋值为null，或者不写该字段

执行后，如下，说明插入一个数据成功了
```
WriteResult({"nInserted":1})

```

2. 批量插入（数组里面）
语法：
```
db.collection.insertMany(
    [<document>, <document>, ....]
    {
        writeConcern:<document>,
        ordered:<boolean>
    }
)
```

### 文档的基本查询
查询数据的语法格式如下：
```
db.collection.find(<query>, [projection])

```
参数：
Parameter|Type|Description
:--|:--:|--:
query|document|可选。使用查询运算符指定选择筛选器。若要返回集合中的所有文档，请忽略此参数或传递空文档({})
projection|document|可选，指定要在与查询筛选器匹配的文档中返回的字段（投影）。若要返回匹配文档中的所有字段，请忽略参数。

【实例】
(1) 查询所有
```
db.comment.find()
或
db.comment.find({})

```
这里你会发现每条文档会有一个叫_id的字段，这个相当于我们原来关系数据库中表的主键，当你在插入文档记录时没有指定该字段，MongoDB会自动创建，其类型是ObjectID类型。

如果我们在插入文档记录时指定该字段也可以，器类型可以是ObjectID类型，也可以是MongoDB支持的任意类型。

如果我想按照一定的条件来查询，比如我想查询userid为1003的记录，怎么办？很简单，只需要在find()中添加参数接口，参数也是json格式。
```
db.commit.find({"userid":"1003"})

```

如果你需要返回符合条件的第一条数据，我们可以使用findOne命令来实现，语法和find一样。
如：查询用户编号是1003的记录，但只最多返回符合条件的第一条记录。
```
db.comment.findone({"userid":"1003"})

```

(2) 投影查询(Projection Query):
如果要查询结果返回部分字段，则需要使用投影查询(不显示所有字段，只显示指定的字段)

如：查询结果只显示_id、userid、nickname:
```
db.comment.find({"userid":"1003"}, {"userid":1, "nickname":1})
```
默认id会显示

如：查询结果只显示userid、nickname，不显示_id
```
db.comment.find({"userid":"1003"}, {"userid":1, "nickname":1, "_id":0})

```

再例如：查询所有数据，但只显示_id、userid、nickname:
```
db.comment.find({}, {"userid":1, "nickname":1})

```


#### try catch
```
try{
    db.commment.insert(
        {

        }
    );
}catch(e){
    print(e)
}
```

### 文档的更新
更新文档的语法：
```
db.collection.update(query, update, options)
//或者
db.collection.update(
    <query>,
    <update>,
    {
        upsert:<boolean>,
        multi:<boolean>,
        writeConcern:<document>,
        collation:<document>,
        arrayFilters:[<filterdocument>,...],
        hint:<document|string>

    }
)
```
参数：
(1) 覆盖的修改
如果我们想修改_id为1的记录，点赞量为1001，输入以下语句：
```
db.comment.update({_id:'1'}, {likenum:NumberInt(1001)})

```
执行后，我们会发现，这条文档除了ikenum字段其他字段都不见了
(2) 局部修改
为了解决这个问题，我们需要使用修改器$set来实现，命令如下：
我们想修改_id为2的记录，浏览量为889，输入以下语句：

```
db.comment.update({_id:"2"}, {$set:{likenum:NumberInt(9988)}})

```
这样就OK了

(3) 批量的修改
更新所有用户为1003的用户昵称为凯撒大帝
```
//默认只修改第一条数据
db.comment.update({}, {$set:{}})
//修改所有符合条件的数据
db.comment.update({}, {$set:{}}, {multi:true})
```
提示：如果不加后面的参数，则只更新符合条件的第一条记录

(3) 列值增长的修改
如果我们想实现对某列值在原有值得基础上进行增加或减少，可以使用$inc运算符来实现。
需求：对3号数据的点赞数，每次递增1
db.comment.update({_id:"3"}, {$inc:{likenum:NumberInt(1)}})

### 删除文档
删除文档的语法解构：
```
db.集合名称.remove(条件)

```

以下语句可以将全部数据全部删除，请慎用
```
db.comment.remove({})
```
如果删除_id=1的记录，输入以下语句
```
db.commentremove({_id:"1"})
```
