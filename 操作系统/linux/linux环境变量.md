# linux环境变量

```sh
# 一般在用户目录下会有几个个隐藏文件如
# .bashrc
# .profile  在.profile中会引入.bashrc
# .bash_profile  优先级最高,存在这个不会执行.profile
# 修改以后重新加载
# 添加path 一般加到文件最后
PATH="$HOME/npm/npm-global/bin:$PATH"
source .bashrc
```

* 三种添加环境变量的方法
  * 临时设置，用 export 指令,如在$PATH中增加JAVA文件夹:
    * $export PATH=$PATH:/usr/local/lib/jdk1.6.0_25
    * 用于当前用户
  * 用户主目录下有一个 .bashrc 隐藏文件，可以在此文件中加入 PATH 的设置如下：export PATH=<你的要加入的路径>:$PATH
  * 多个路径 export PATH=<你要加入的路径1>:<你要加入的路径2>: ...... :$PATH
  * 所有用户
    * vi /etc/profile
    * export PATH=<你要加入的路径>:$PATH
* 修改环境变量后，除了第一种方法立即生效外，第二第三种方法要立即生效，可以source ~/.bashrc或者注销再次登录后就可以了！
* echo $PATH 可以查看环境变量
