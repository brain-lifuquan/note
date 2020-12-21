# mongo权限管理

> mongodb的用户名和密码是基于特定数据库的，而不是基于整个系统的。所有数据库db都需要设置密码<br>

* 设置管理用户和密码

```sh
# 进入admin数据库
> use admin
# 创建管理员账号
> db.createUser({ user: "useradmin", pwd: "adminpassword", roles: [{ role: "userAdminAnyDatabase", db: "admin" }] })
# mongodb中的用户是基于身份role的
# 该管理员账户的 role是 userAdminAnyDatabase
# ‘userAdmin’代表用户管理身份，’AnyDatabase’ 代表可以管理任何数据库。
# 验证第3步用户添加是否成功
> db.auth("useradmin", "adminpassword") 
# 如果返回1，则表示成功。
```

* 创建其他数据库管理员账号

```sh
# 使用系统管理员账号登录
> use admin
> db.auth("useradmin", "adminpassword") 
# 新技安需要管理的mongodb数据的账号密码
> use yourdatabase
> db.createUser({ user: "youruser", pwd: "yourpassword", roles: [{ role: "dbOwner", db: "yourdatabase" }] })
# dbOwner 代表数据库所有者角色，拥有最高该数据库最高权限。
# 创建数据库读写账户
> db.createUser({ user: "youruser2", pwd: "yourpassword2", roles: [{ role: "readWrite", db: "yourdatabase" }] })
# 该用户用于该数据的读写，只拥有读写权限。
> db.createUser({ user: "youruser2", pwd: "yourpassword2", roles: [{ role: "Read", db: "yourdatabase" }] })
# 该用户只有只读权限
```

* 内建角色
  * Read：允许用户读取指定数据库
  * readWrite：允许用户读写指定数据库
  * dbAdmin：允许用户在指定数据库中执行管理函数，如索引创建、删除，查看统计或访问system.profile
  * userAdmin：允许用户向system.users集合写入，可以找指定数据库里创建、删除和管理用户
  * clusterAdmin：只在admin数据库中可用，赋予用户所有分片和复制集相关函数的管理权限。
  * readAnyDatabase：只在admin数据库中可用，赋予用户所有数据库的读权限
  * readWriteAnyDatabase：只在admin数据库中可用，赋予用户所有数据库的读写权限
  * userAdminAnyDatabase：只在admin数据库中可用，赋予用户所有数据库的userAdmin权限
  * dbAdminAnyDatabase：只在admin数据库中可用，赋予用户所有数据库的dbAdmin权限。
  * root：只在admin数据库中可用。超级账号，超级权限
