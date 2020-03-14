---
noteId: "85c38a205de511ea853dad8db01a0bd6"
tags: []

---

### 索引-Index
#### 概述
> 索引支持在MongoDB中搞笑的执行查询。如果没有索引，MongoDB必须执行拳击和扫描，即臊面集合中的每一个文档，一选择与查询语句匹配的文档，这种扫描全集合的查询效率是非常低的，特别是在处理大量的数据时，查询可以话费几十秒甚至几分钟，这对网站的性能是非常致命的。

> 如果查询存在适当的索引，MongoDB可以使用该索引限制必须检查的文档数。

> 索引是特殊的数据结构，它以易于遍历的形式存储集合数据集的一小部分。索引存储特定字段或子组字段的值，按字段值排序。索引项的排序支持有效的相等匹配和基于范围的查询操作。此外，MongoDB还可以使用索引中的排序返回排序结果。

了解：
MongoDB索引使用B树数据结构（确切的说是B-Tree， MySQL是B+Tree）

#### 索引的类型
1. 单字段索引
MongoDB支持在文档的单个字段上创建用户定义的升序/降序索引，成为单字段索引（Single Field Index）
对于单个字段索引和排序操作，索引键的排序顺序（升序或者降序）并不重要，因为MongoDB可以在任何方向上遍历索引。

{score:1} Index

2. 复合索引
MongoDB还支持多个字段的用后定义索引，即复合索引（Compound Index）。
复合索引中列出的字段顺序具有重要意义。例如，如果复合索引由{userid:1, score:-1}组成，则索引首先按userid正序排序，然后在每一个userid的值内，再按score倒序排序

{userid:1, score:-1} Index

3. 其他索引
地理空间索引(Geospatial Index), 文本索引(Text Indexes), 哈希索引(Hashed Indexes)

地理空间索引
为了支持对地理空间坐标数据的有效查询，MongoDB提供了两种特殊的索引：返回结果时使用平面几何的二维索引和返回结果时使用球面几何的二维球面索引

文本索引(Text Indexes)
MongoDB提供了一种文本索引类型，支持在集合中搜索字符串内容。这些文本索引不存储特定于语言的停止词(例如"the", "a", "or")，而集合中的词作为词干，只存储根词。

哈希索引(Hashed Indexes)
为了支持基于散列的分片，MongoDB提供了散列索引内省，它对字段值的散列进行索引。这些索引在其范围内的值分布更加速记，但只支持相等匹配，不支持基于范围的查询。

#### 索引的管理操作
1. 查看
说明：
返回一个集合中的所有索引的数组
语法：
```
db.collection.getIndexes()

```
提示：该语法命令运行要求是MongoDB 3.0+
【示例】
查看comment集合中的所有索引情况
```
db.commit.getIndexes()
[
        {
                "v" : 2, 版本
                "key" : {
                        "_id" : 1   升序
                },
                "name" : "_id_",    索引的名称
                "ns" : "articledb.commit"   存放地址
        }
]

```
结果中默认显示的是_id索引。

#### 索引的创建
说明：
在集合上创建索引
语法：
```
db.collectiion.createIndex(key, options)

```
参数：
Parameter|Type|Description
:--|:--:|--:
keys|document|包含字段和值对的文档，其中字段是索引键，值描述该字段的索引类型。对于字段上的升序索引，请指定值1; 对于降序索引，请指定值-1.比如：{字段：1或-1}, 其中1为按升序创建索引，-1为降序
options|document|可选。包含一组控制索引创建的选项的文档。油管详细信息，请参见选项详情列表

options(更多选项)列表：
常用的unique

(1) 单字段索引实例：对userid字段建立索引：


#### 索引的移除
说明：可以移除指定的索引，或移除所有索引
一、 指定索引的移除
语法：
```
db.collection.dropIndex(index)

```
参数：
Parameter|Type|Description
:--|:--:|--:
index|string or document|指定要删除的索引。可以通过索引名称或索引规范文档指定索引。若要删除文本索引，请指定索引名称。

【示例】
删除comment集合中userid字段上的升序索引：
```
db.comment.dropIndexes()

```
成功删除

所有索引的移除
```
db.comment.dropIndex()

```

### 索引的使用
#### 执行计划
分析查询性能(Analyze Query Performance)通常使用执行计划(解释计划、Explqin Plan)来查看查询的情况，如查询耗费的时间、是否基于索引查询等。

那么，通常，我们想知道，建立的索引是否有效，效果如何，都需要通过执行计划查看。
语法：
```
db.collection.find(query, ooptions),explain(options)

```

#### 涵盖的查询
Covered Queries
当查询条件和查询的投影仅包含索引字段时，MongoDB直接从索引返回结果，而不扫描任何文档或将文档带入内存，这些覆盖的查询可以非常有效









