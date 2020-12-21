# MongoDB

## 文档地址

> 官方文档: <https://docs.mongodb.com/manual/tutorial/query-documents/>

## Pymongo

### database

> [文档地址](https://pymongo.readthedocs.io/en/stable/api/pymongo/database.html)

* 方法

```python
# 登录
authenticate(name=None, password=None, source=None, mechanism='DEFAULT', **kwargs)
logout()
# 查看全部表名
collection_names(include_system_collections=True, session=None)
# 列出表名 可筛选
list_collection_names(session=None, filter=None, **kwargs)
# 示例 列出所有的非system的表
filter = {"name": {"$regex": r"^(?!system\.)"}}
db.list_collection_names(filter=filter)
# 返回一个只想全部表的cursor
list_collections(session=None, filter=None, **kwargs)
# 创建表
create_collection(name, codec_options=None, read_preference=None, write_concern=None, read_concern=None, session=None, **kwargs)
# 删除表
drop_collection(name_or_collection, session=None)
# 检查表
validate_collection(name_or_collection, scandata=False, full=False, session=None, background=None)
# 获取一个表的实例
get_collection(name, codec_options=None, read_preference=None, write_concern=None, read_concern=None)
# 执行mongo指令
command(command, value=1, check=True, allowable_errors=None, read_preference=None, codec_options=CodecOptions(document_class=dict, tz_aware=False, uuid_representation=UuidRepresentation.PYTHON_LEGACY, unicode_decode_error_handler='strict', tzinfo=None, type_registry=TypeRegistry(type_codecs=[], fallback_encoder=None)), session=None, **kwargs)
# 监视数据库
watch(pipeline=None, full_document=None, resume_after=None, max_await_time_ms=None, batch_size=None, collation=None, start_at_operation_time=None, session=None, start_after=None)
# 示例
with db.watch() as stream:
    for change in stream:
        print(change)
```

### cursor

> 文档地址:<https://pymongo.readthedocs.io/en/stable/api/pymongo/cursor.html>

* 方法

```python
# 限制一批中返回的文档数。每批处理都需要往返服务器。可以对其进行调整以优化性能并限制数据传输
batch_size(batch_size)
```

### collection

> 文档地址:<https://pymongo.readthedocs.io/en/stable/api/pymongo/collection.html>

* 方法:

```python
# 查询
# 返回值为一个Cursor(游标)实例
find(filter=None, projection=None, skip=0, limit=0, no_cursor_timeout=False, cursor_type=CursorType.NON_TAILABLE, sort=None, allow_partial_results=False, oplog_replay=False, modifiers=None, batch_size=0, manipulate=True, collation=None, hint=None, max_scan=None, max_time_ms=None, max=None, min=None, return_key=False, show_record_id=False, snapshot=False, comment=None, session=None)
# 查询示例
# 以正则表达式匹配数据查询
records = list(col.find({'天': {'$regex':'^2020-09-30'}}))
records = list(COL.find(
    {
        '日期': {'$gte': day}
    },
    {
        '_id': 0,
        '日期': 1,
        '华为: VoLTE上行吞字断续比例': 1,
        '诺基亚: VoLTE上行吞字断续比例': 1,
        '其他: VoLTE上行吞字断续比例': 1,
    }
))
# 返回一条记录
find_one(filter=None, *args, **kwargs)
find_one_and_delete(filter, projection=None, sort=None, hint=None, session=None, **kwargs)
# 查找并替换一条
find_one_and_replace(filter, replacement, projection=None, sort=None, return_document=ReturnDocument.BEFORE, hint=None, session=None, **kwargs)
# 查找并更新一条
find_one_and_update(filter, update, projection=None, sort=None, return_document=ReturnDocument.BEFORE, array_filters=None, hint=None, session=None, **kwargs)

# 返回记录数 filter为必选
# 此方法会触发mongo server中进行查询, 对于大表会很耗内存和时间,需要慎用
count_documents(filter, session=None, **kwargs)
# 返回估算的记录数 不带参数返回集合的记录总数
# 该方法并不触发对集合的查询过程,估算使用的是集合的metadata 元数据
estimated_document_count(**kwargs)

# 返回按照key不重复的记录
distinct(key, filter=None, session=None, **kwargs)

# 批量插入
insert_many(documents, ordered=True, bypass_document_validation=False, session=None)

# 索引操作
create_index(keys, session=None, **kwargs)

# 批量删除
delete_many(filter, collation=None, hint=None, session=None)
# 删除一条
delete_one(filter, collation=None, hint=None, session=None)
```

```python
# 创建连接
from pymongo import MongoClient
mymongo = MongoClient('mongodb://localhost:27017/')
# 定位到数据库
db = mymongo['npo']
# 定位到数据集合
col = db['SEQ_VoLTE吞字断续统计数据']
# 选择collection
col = mongo['npo']['VoLTE音视频质量报表详情']
# 删除collection
db['SEQ_VoLTE吞字断续统计数据'].drop()
# 查询index
db['SEQ指标趋势'].list_indexes()
# 创建index
db['SEQ指标趋势'].create_index([('name', pymongo.ASCENDING)], unique=True)
# 插入数据
# 插入1条数据
insert_one()
# 一次插入多条数据
insert_many()
```

## 术语

|SQL术语|MongoDB|术语解释/说明|
|---|---|---|
|database|database|数据库|
|table|collection|数据库表/集合|
|row|document|数据记录行/文档|
|column|field|数据字段/域|
|index|index|索引|
|table|joins|表连接,MongoDB不支持|
|primary key|primary key|主键,MongoDB自动将_id字段设置为主键|

## 查询数据

db.collection.find(query, projection)

|操作|格式|范例|RDBMS中的类似语句|
|---|---|---|---|
|等于|`{<key>:<value>}`|db.col.find({"by":"菜鸟教程"}).pretty()|where by = '菜鸟教程'|
|小于|`{<key>:{$lt:<value>}}`|db.col.find({"likes":{$lt:50}}).pretty()|where likes < 50|
|小于或等于|`{<key>:{$lte:<value>}}`|db.col.find({"likes":{$lte:50}}).pretty()|where likes <= 50|
|大于|`{<key>:{$gt:<value>}}`|db.col.find({"likes":{$gt:50}}).pretty()|where likes > 50|
|大于或等于|`{<key>:{$gte:<value>}}`|db.col.find({"likes":{$gte:50}}).pretty()|where likes >= 50|
|不等于|`{<key>:{$ne:<value>}}`|db.col.find({"likes":{$ne:50}}).pretty()|where likes != 50|

<hr>
