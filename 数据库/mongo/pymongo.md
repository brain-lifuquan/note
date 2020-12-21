# pymongo

> 官方文档: <https://pymongo.readthedocs.io/en/stable/api/index.html>

## mongo_client mongo客户端

```python
from pymongo import MongoClient
# 创建一个需要auth的本地mongoclient实例
mongo = MongoClient(username='admin', password='fantuan1985')
# 获取mongo的全部数据库名的列表
mongo.database_names()
```

## database 数据库

```python
# 获取一个名为daily的数据库的实例
# daily可以是已存在的数据库或不存在的数据库
db = mongo.get_database('daily')
# mongo的用户控制是针对每个数据库的,对于新增的数据库,如果不添加用户是无法连接的
db.add_user(name='admin', password='fantuan1985')
# 使用添加的用户登陆到数据库
db.authenticate(name='admin', password='fantuan1985')
# 获取数据库中全部的集合的名字列表
db.collection_names()
```

## colection 集合

```python
# 创建一个集合,并返回集合的实例
col = db.create_collection("4G闭锁小区1")
# 获取一个集合的实例,可以是不存在的集合
col = db.get_collection("4G闭锁小区1")
```

## index 索引

```python
import pymongo
# 在集合上创建索引
# pymongo.ASCENDING 表示正序
col.create_index([("_enbid", pymongo.ASCENDING), ("_lcrid", pymongo.ASCENDING)], name="4G闭锁小区_enbid_lcrid_unique", unique=True)
```
