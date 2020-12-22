# 引擎使用

## 基本用法

* 典型用法 create_engine() 是每个特定数据库URL一次，在单个应用程序进程的生命周期内全局保存
* 通常要求 Engine 用于每个子进程.这是因为 Engine 维护对最终引用DBAPI连接的连接池的引用-这些连接通常不可跨进程边界移植。
  * Engine 配置为不使用池（通过使用 NullPool ）没有此要求

* 可以直接用于向数据库发出SQL。最通用的方法是首先获取连接资源，通过 Engine.connect() 方法
  * 连接是的实例 Connection ，这是一个 代理 实际DBAPI连接的对象。DBAPI连接从连接池中的 Connection 创建。
  * 返回的结果是 ResultProxy 它引用了DBAPI游标，并提供了与DBAPI游标基本兼容的接口
    * 光标将由 ResultProxy 当它的所有结果行（如果有）用完时释放
    * 如果ResultProxy不返回任何行,例如UPDATE语句的行（不返回任何行），在构造后立即释放光标资源

```python
connection = engine.connect()
result = connection.execute("select username from users")
for row in result:
    print("username:", row['username'])
connection.close()
# 可以通过使用 Engine本身的execute() 方法
# 无连接执行
result = engine.execute("select username from users")
for row in result:
    print("username:", row['username'])
```

## 使用事务

* Connection 对象提供 begin() 返回 Transaction 对象。此对象通常在try/except子句中使用，以确保它可以调用 Transaction.rollback() 或 Transaction.commit() ：：

```python
connection = engine.connect()
trans = connection.begin()
try:
    r1 = connection.execute(table1.select())
    connection.execute(table1.insert(), col1=7, col2='this is some data')
    trans.commit()
except:
    trans.rollback()
    raise
```

* 使用上下文管理器

```python
# runs a transaction
with engine.begin() as connection:
    r1 = connection.execute(table1.select())
    connection.execute(table1.insert(), col1=7, col2='this is some data')
# trans可用
with connection.begin() as trans:
    r1 = connection.execute(table1.select())
    connection.execute(table1.insert(), col1=7, col2='this is some data')
```
