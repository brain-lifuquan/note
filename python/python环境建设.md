# python环境建设

## pip

### 常用命令

* `pip install -r requirements.txt`按照文件中的列表安装

```sh
# 生成requirements.txt文件
pip freeze > requirements.txt
# 安装requirements.txt依赖
pip install -r requirements.txt
```

* `pip show jupyter` 显示某个库的安装信息

### 在Linux系统使用pip(系统变量)

* 通常我们使用的并不是root用户
* 使用用户pip安装的时候 对系统python目录没有权限
* 此时会有提示`Defaulting to user installation because normal site-packages is not writeable`
* 此时pip安装的路径为用户目录下的`.local/lib/python3.9/site-packages`
* 需要将此目录添加到PYTHONHOME环境变量,才能正常导入
  * 临时添加`export PYTHONPATH=~/.local/lib/python3.9/site-packages`
  * 永久添加 修改并重新加载用户的 .bashrc文件
  * `vi ~/.bashrc`
  * 在最后添加一行 `PYTHONPATH=~/.local/lib/python3.9/site-packages`
  * 保存重新加载文件`source ~/.bashrc`

> The script pyflakes is installed in '/home/lfq/.local/bin' which is not on PATH.<br>
> 在ubuntu中使用pip安装 报出来的, 应该是需要把这个目录添加到PATH来解决<br>
> 因为安装python模块的时候会同时安装一些可执行程序, 现在的提示的是安装的可执行程序不在系统的PATH目录,无法直接通过命令执行<br>

### pip镜像加速

* 直接使用国内镜像安装

```sh
# 使用命令方式选择镜像地址进行安装
pip install redis -i https://pypi.douban.com/simple
```

* 使用配置文件一劳永逸
  * `linux` 用户目录下 `~/.pip/pip.conf`
  * `win10` 用户目录下 `/pip/pip.ini`

```conf
[global]
timeout = 6000
index-url = http://pypi.douban.com/simple/
[install]
use-mirrors = true
mirrors = http://pypi.douban.com/simple/
trusted-host = pypi.douban.com
```

* 国内镜像地址
  * 阿里云：[http://mirrors.aliyun.com/pypi/simple/](http://mirrors.aliyun.com/pypi/simple/)
  * 豆瓣：[http://pypi.douban.com/simple/](http://pypi.douban.com/simple/)
  * 清华大学：[https://pypi.tuna.tsinghua.edu.cn/simple/](https://pypi.tuna.tsinghua.edu.cn/simple/)
  * 中国科学技术大学：[http://pypi.mirrors.ustc.edu.cn/simple/](http://pypi.mirrors.ustc.edu.cn/simple/)
  * 华中科技大学：[http://pypi.hustunique.com/](http://pypi.hustunique.com/)
