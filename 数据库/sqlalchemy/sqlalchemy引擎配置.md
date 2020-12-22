# sqlalchemy引擎配置

## SQLite

```python
# sqlite://<nohostname>/<path>
# 相对路径
# where <path> is relative:
engine = create_engine('sqlite:///foo.db')
# 绝对路径
# Unix/Mac - 4 initial slashes in total
engine = create_engine('sqlite:////absolute/path/to/foo.db')
# Windows
engine = create_engine('sqlite:///C:\\path\\to\\foo.db')
# Windows alternative using raw string
engine = create_engine(r'sqlite:///C:\path\to\foo.db')
# SQLite :memory: 数据库
engine = create_engine('sqlite://')
```

## oracle

```python
engine = create_engine('oracle://scott:tiger@127.0.0.1:1521/sidname')
engine = create_engine('oracle+cx_oracle://scott:tiger@tnsname')
```

## PostgreSQL

```python
# default
engine = create_engine('postgresql://scott:tiger@localhost/mydatabase')
# psycopg2
engine = create_engine('postgresql+psycopg2://scott:tiger@localhost/mydatabase')
# pg8000
engine = create_engine('postgresql+pg8000://scott:tiger@localhost/mydatabase')
```

## MySQL

```python
# default
engine = create_engine('mysql://scott:tiger@localhost/foo')
# mysqlclient (a maintained fork of MySQL-Python)
engine = create_engine('mysql+mysqldb://scott:tiger@localhost/foo')
# PyMySQL
engine = create_engine('mysql+pymysql://scott:tiger@localhost/foo')
```

## Microsoft SQL

```python
# pyodbc
engine = create_engine('mssql+pyodbc://scott:tiger@mydsn')
# pymssql
engine = create_engine('mssql+pymssql://scott:tiger@hostname:port/dbname')
```
