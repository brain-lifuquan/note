# mongo安装

## docker安装

```bash
# 获取image
$ docker pull mongo:4.4
# 运行一个容器 将默认的配置文件拷贝出来
$ docker run --name mongo -d mongo:4.4
$ sudo docker cp mongo:/etc/mongod.conf.orig ~/dbdata/mongo/config
$ docker stop mongo
$ docker rm mongo
# 修改配置文件
# 按照需求修改配置文件
# 重新运行容器,加载本地目录下的配置文件及数据目录
$ docker run --name mongo -v ~/dbdata/mongo/data:/data/db -v ~/dbdata/mongo/config/mongod.conf.orig:/etc/mongod.conf.orig -p 27017:27017 -d mongo:4.4 --auth
# --auth 要求要用密码登录
# 进入mongo容器
$ docker exec -it mongo bash
# 进入mongo数据库
root@1b3ada58f6bd:/# mongo
# 进入admin数据库
> use admin
switched to db admin
# 创建一个名为 admin，密码为 123456 的用户
> db.createUser({ user:'admin',pwd:'yourpassword',roles:[ { role:'userAdminAnyDatabase', db: 'admin'}]});
# 试用新创建的账号
> db.auth('admin', '123456')
1
# 退出mongo
> exit
bye
# 退出容器
exit
```

### win10安装mongodb

* 4.4版本直接运行安装包即可启动服务

* 运行安装包 mongodb-win32-x86_64-2008plus-ssl-3.6.20-signed.msi
* 安装服务
  * 安装前需要先创建 "C:\Program Files\MongoDB\data\log\mongodb.log" 文件
  * 执行命令 `"C:\Program Files\MongoDB\Server\3.6\bin\mongod.exe" --logpath "C:\Program Files\MongoDB\data\log\mongodb.log" --logappend --dbpath "C:\Program Files\MongoDB\data" --directoryperdb --serviceName MongoDB --install`
  * 启动服务 `net start MongoDB`

* mongo 可执行文件目录 `C:\Program Files\MongoDB\Server\3.6\bin`
  * mongod.exe 服务端
  * mongo.exe 客户端
* 备份和恢复
  * mongodump.exe 备份
    * `mongodump -h dbhost -d dbname -o dbdirectory`
    * 不带参执行 将当前库导出到与mongodump同目录下的dump文件夹中
    * 可选参数 --host --port --dbpath --out --collection --db
  * mongorestore 恢复
    * `mongorestore -h <hostname><:port> -d dbname <path>`
    * 不带参执行 从与mongorestore同目录下的dump文件夹中恢复数据

### wsl安装

* wsl1安装未成功
* wsl2安装未尝试(win10 2004版本稳定性太差)
