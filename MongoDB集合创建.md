---
noteId: "1ea84c405d5911ea842439931df45c97"
tags: []

---
### 集合的创建
#### 集合的显式创建（了解）
基本语法格式:
```
db.createCollection(name)

```
参数说明：
    - name：要创建的集合名称

例如：创建一个名为mycollection的普通集合
```
db.createCollection("mycollection")

```
查看当前库中的表：show tables命令
```
show collections
或
show tables

```
集合的命名规范
    - 集合名称不能是空字符串""
    - 集合名不能含有\0字符（空字符），这个字符表示集合名的结尾
    - 集合名不能以"system"开头，这是为系统集合保留的前缀。

#### 集合的隐式创建
当向一个集合中插入一个文档的时候，如果集合不存在，则会自动创建集合。
详见文档的插入章节。
提示：通常我们使用隐式创建文档即可。

3.3.3集合的删除
集合删除语法格式如下：
```
db.collection.drop()
或
db.集合.drop()
```
返回值
如果从公删除选定集合，则drop()方法返回true，否则返回false
例如：要删除mycollection集合
```
db.mycollection.drop()
```



