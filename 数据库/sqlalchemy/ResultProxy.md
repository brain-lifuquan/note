# class sqlalchemy.engine.ResultProxy(context)

* 包装db-api光标对象，以便更容易地访问行列。
  * 可以通过其整数位置、不区分大小写的列名或 schema.Column 对象。
* ResultProxy 还处理结果列数据的后处理，使用 TypeEngine 对象，从生成此结果集的原始SQL语句中引用
