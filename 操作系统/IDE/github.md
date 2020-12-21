# github

## 分支操作

```sh
# 创建分支
git branch dev
# 切换分支
git checkout dev
# 删除分支
git branch --delete dev
git branch -d dev
# 强制删除
git branch --delete --force dev
# 查看分支默认为本地分支
git branch
# 查看全部分支 包含远程分支
git branch --all
# 合并分支(将dev合并到当前分支)
git merge dev
```

```sh
# 回退/ 撤销
git reset --hard aa708
        --hard  删除提交记录并不保存所删除记录所做的更改
        --soft  删除了提交记录，但是还保存了提交所做的更改
        git reset --hard HEAD   git reset --hard HEAD^  回退到前一版本
        git reset --hard ad2080c    回退到对应版本
# 撤销文件未提交的修改
git checkout src/com/jay/example/testforgit/MainActivity.java  
# 先撤销添加,再撤销修改
git reset HEAD src/com/jay/example/testforgit/MainActivity.java
git checkout src/com/jay/example/testforgit/MainActivity.java
# 查看 commit记录
git log
# 退出gitlog
q
# 命令记录
git reflog
# 检查状态
git status
```

## 远端操作

```sh
# 本地没有.git仓库,直接从git_hub远端库拉取的情况
git clone git@github.com:brain-lifuquan/myvue.git
# 本地会自动添加.git仓库
# 本地会自动添加一个名为origin的远端仓库

# 从在线的库上获取最新的修改信息而不需要合并代码
git fetch

# 测试网络
ssh -T git@github.com

# 添加远程库
# origin 为远程库命名,可以自己定义
git remote add origin 远程库地址
# 克隆远程库
git clone 远程库地址
# 查看已关联的远程库
git remote
# 从远程库拉取数据
# origin 是远程库命名 master 是远程库分支
git pull origin master
# 推送代码到远程库
git push origin master
# 删除远程库分支
git push origin --delete dev
```

```sh
# git pull 时的提示信息
[lfq@localhost 编程基础]$ git pull origin master
warning: 不建议在没有为偏离分支指定合并策略时执行 pull 操作。 您可以在执行下一次
pull 操作之前执行下面一条命令来抑制本消息：

  git config pull.rebase false  # 合并（缺省策略）
  git config pull.rebase true   # 变基
  git config pull.ff only       # 仅快进

您可以将 "git config" 替换为 "git config --global" 以便为所有仓库设置
缺省的配置项。您也可以在每次执行 pull 命令时添加 --rebase、--no-rebase，
或者 --ff-only 参数覆盖缺省设置。
```

## 配置

```sh
# 全局设置
git config --global user.name "lifuquan"
git config --global user.email "lifuquan@foxmail.com"
# 生成公钥和私钥
ssh-keygen -t rsa -C "lifuquan@foxmail.com"
```

### 链接github库流程

* 在github上创建新库
  * 建议创建时不要选择复选框
* 创建完成后页面上会有命令提示

* 设置default分支
  * 仓库==>settings==>branches==>选择branche==>update

* 如果提示连接失败,可能的原因是未在github上添加ssh的公钥
  * 用户图标--> settings --> 设置页面
  * SSH and GPG keys --> SSH keys --> New SSH key
  * 粘贴公钥内容

```sh
# 提示错误信息
git@github.com: Permission denied (publickey).
fatal: Could not read from remote repository.
# 生成公钥和私钥
ssh-keygen -t rsa -C "lifuquan@foxmail.com"
# 默认的生成目录是 用户家目录下 .ssh 目录
# 目录下包含一个私钥文件 一个公钥文件 id_rsa.pub
vi /root/.ssh/id_rsa.pub
# 复制公钥内容 添加到githb.com对应位置
```

* Git:failed to execute git
  * 原因: git没有设置用户信息

* *设置部分文件夹不向远程库同步*

* 在文件夹下添加.gitignore文件
  * .gitignore 不一定放在主目录下, 可以放在子目录中
* 在.gitignore文件中添加不想同步的文件夹相对路径
  * build/
  * dist/

## 下载安装

* 下载地址
  * <https://git-scm.com/download>
  * <https://mirrors.edge.kernel.org/pub/software/scm/git/>

* LF will be replaced by CRLF   LF和CRLF都是换行符，在各操作系统下，换行符是不一样的，Linux/UNIX下是LF,而Windows下是CRLF
  * `git config --global core.autocrlf false`

* 解决中文乱码问题
  * 添加环境变量 LESSCHARSET utf-8
  * `git config --global i18n.commitencoding utf-8` 提交命令的时候使用utf-8编码集提交
  * `git config --global i18n.logoutputencoding utf-8` 表示日志输出时使用utf-8编码集显示

## 使用.gitignore文件进行屏蔽

> `gitignore`文件的作用就是告诉Git哪些文件不需要添加到版本管理中,被过滤掉的文件不会出现在你的GitHub库中了，当然本地库中还有，只是push的时候不会上传。

* 指定被过滤掉的文件
  * /mtk/ 过滤整个文件夹
  * *.zip 过滤所有.zip文件
  * /mtk/do.c 过滤某个具体文件

* 指定哪些文件添加到版本管理(不被过滤)
  * !src/   不过滤该文件夹
  * !*.zip   不过滤所有.zip文件
  * !/mtk/do.c 不过滤该文件

* 配置方法
  * 以斜杠/开头表示目录；
  * 以星号*通配多个字符；
  * 以问号?通配单个字符
  * 以方括号[]包含单个字符的匹配列表；
  * 以叹号!表示不忽略(跟踪)匹配到的文件或目录
* git 对于 .ignore 配置文件是按行从上到下进行规则匹配的，意味着如果前面的规则匹配的范围更大，则后面的规则将不会生效；

* 示例
  * 规则：fd1/*
    * 说明：忽略目录 fd1 下的全部内容；注意，不管是根目录下的 /fd1/ 目录，还是某个子目录 /child/fd1/ 目录，都会被忽略；
  * 规则：/fd1/*
    * 说明：忽略根目录下的 /fd1/ 目录的全部内容；

```git
# 忽略全部内容，但是不忽略 .gitignore 文件、根目录下的 /fw/bin/ 和 /fw/sf/ 目录；
/*
!.gitignore
!/fw/bin/
!/fw/sf/
```

## github术语解析

### Blame

Git中的“blame”特性描述了文件的每一行的最近一次修改信息，包括修改内容、作者和时间等；可用于追踪某软件新特性的添加及引起bug的提交操作；

### Repository

库是GitHub的最基本元素，可想象成本地的项目文件夹；一个库包含所有的项目文件(包括帮助文档)，并保存每个文件的修改历史；库可以有多个合作开发者，也可以作为公共库或私有库的形式开发；

#### Private Repository

私有库，是指只能被库的创建者或者合作开发者查看并编辑的库，需要付费使用；

### Pull Request

即代码合并请求，由其它开发者或用户向项目的collaborators提议的修改请求，collaborators觉得修改信息合理有效即接受，否则拒绝；

### Merge

将一个分支中的修改内容应用到另一个分支的操作就做合并；若两个分支内的修改内容无冲突，则可以通过合并请求(a Pull Request)或命令行(the command line)完成合并操作；

### Clone

克隆，是将GitHub上的库文件整个复制到本地主机上，可以实现离线修改，等上线后再同步至Github上的库即可；

### Fork

对其它开发者的库的个人复制，复制的库存在你自己的账户上，你可以自行修改项目内容而不会影响原始的库，也可以将自己的修改通过合并请求(a pull request)的方式请求原始库的开发者更新你的修改；

### Push

推送，表示将本地的修改内容推送至线上的库，这样其它的开发者就可以通过GitHub网站访问到你的修改内容了；

### Remote

远端版本，即类似于GitHub.com的非本地主机的项目版本，可以连接至本地克隆的版本以实现内容同步；

### Open Source

开源，原指可自由使用、修改和传播的软件，现扩展为一种超越软件的合作哲学，即工件(working materials)在线可用，可被任何人复制(fork)、修改(modify)、讨论(discuss)、并提出修改意见(contribute to)；

### Upstream

上游，对于一个branch或者fork来说，源库的主分支即是其它修改信息的源头，被称为upstream，相对的其它branch或fork就被称为downstream了；

### 版本

#### Commit

提交信息，或者称为修改信息，是个人提交的对文件的修改记录；

#### Diff

差异，指2个commit或保存的改变间的差异，可以很直观的看出一个文件自上次commit后增加或删除的内容；

#### Branch

分支是一个库的并行版本，包含在库内，允许独立的开发而不影响现有主分支(primary or master)的运行；当在分支的修改需要发布时，就可以将分支合并(merge)至主分支(master branch)，这样利于多人的分布式开发；

### 人员组织

#### User

用户，指个人注册的GitHub账户，每个用户都可以拥有多个公共库或私有库，也可被邀请加入organizations或称为collaborates；

#### Organizations

组织，即多个开发者组成的团体，可包含众多的库和开发团队；

#### Collaborator

合作开发者，被库的所有者邀请共同开发某一项目，拥有对库的读写权限；

#### Contributor

贡献者，对项目有所贡献(如提交代码，修复bug等)的开发者，但不具备合作开发者的访问权限；

### SSH Key

私钥，是GitHub用以验证你本地主机的身份的，并用此密钥加密传输GitHub网站和你本地主机的数据传输，以保证安全性

### Fetch

取回，表示从在线的库上获取最新的修改信息而不需要合并代码，取回的代码可以与你本地的分支代码进行比较；
