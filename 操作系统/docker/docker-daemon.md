# docker daemon 守护进程

* Docker Daemon是Docker的守护进程, Docker Client通过命令行与Docker Damon通信,完成Docker相关操作

## 修改Docker Daemon的方式

* 命令行修改 只在当前有效,重启会失效
* 修改启动项 配置稳定,不经常修改的配置项可以放在启动项中
* 修改配置文件 需要经常修改,又需要一直生效的选项放在配置文件

* 在启动Docker Daemon时可以在命令行后追加配置项,立即生效
  * 例如 -D/--debug(开启或关闭调试模式,默认关闭)

* 修改Docker Daemon启动项
  * 通过systemctl命令启动/关闭/加入自启动 docker操作的就是Docker Daemon
  * 例如`systemctl enable docker`将Docker Daemon加入自启动
  * Docker启动时,会从启动文件中(/etc/systemd/system/docker.service.d目录下)读取启动项, 如果没有目录可以手工创建该目录并在目录下创建docker.conf文件
  * 在docker.conf文件中添加docker daemon启动项

```sh
# 修改配置文件后需要重新加载配置文件并重启docker
systemctl daemon-reload
systemctl start docker
```

* 自定义Docker Daemon配置文件
  * docker启动时,从启动文件读取启动项之后会追加配置文件中的配置项
  * 配置文件默认目录 /etc/docker/daemon.json
  * 通过--config-file可以设置Docker Daemon的配置文件路径
  * 配置文件中的配置项不能和Docker Daemon启动项重复,一旦重复,会报错无法启动
  * 配置文件为JSON格式
  * 某些配置项支持动态加载(通过信号量实现),不用重启Docker就可以更新配置

## 配置项

* 仓库相关配置
  * --disable-legacy-registry
    * Docker从1.6版本开始，就支持从V2版本镜像仓库下载镜像，为了兼容会先尝试连接镜像仓库的V2接口，如果失败则再次尝试V1接口
    * 一旦设置了–disable-legacy-registry后就不会连接镜像中的V1接口，如果连接V2接口失败则立即退出
  * --registry-mirror
    * 通过设置本地的镜像地址,提升下载和上传速率
    * 加速器地址： <https://hk8nhg9w.mirror.aliyuncs.com>
  * --insecure-registry
    * Docker将镜像仓库分为安全和不安全的，从不安全的镜像仓库下载、上传及搜索镜像都会失败
    * 不安全的镜像可能是没有使用TLS或者主机上没有保存BA证书
    * 如果镜像仓库没有设置TLS,从中下载就需要添加到此列表中

```json
{
  "registry-mirrors": ["https://hk8nhg9w.mirror.aliyuncs.com"]
}
```

* 安全相关配置
  * --host/H
    * Docker Daemon 和Docker Client通过Socket方式通信
    * Docker Daemon监听Socker的方式有三种:unix tcp fd
    * 默认情况下在同一台主机上使用本地socket通信,配置为unix:///var/run/docker.sock
    * 当daemon和client运行在不同主机上时, 需要daemon开启TCP Socket,如果没有启动--tls,则daemon和client之间的通信没有任何认证加密,可以通过-H设置在某个ip上监听
    * 当主机有多个网卡时,通过docker daemon -H tcp://0.0.0.0:2375在主机的所有ip上监听,端口2375.如果将0.0.0.0替换为指定ip,则只在指定ip上监听.
  * --tls
    * daemon和client远程通信时需要使用TLS进行安全加密
    * 加入TLS相关配置后分别添加服务器CS证书和客户端CA证书
    * daemon只接受CA认证的客户端发送的命令

```json
// 开放远程服务
"hosts": ["tcp://0.0.0.0:2375", "unix:///var/run/docker.sock"]
```

```sh
# 测试远程连接
$ curl 127.0.0.1:2375/version
# 打开防火墙端口，外部就可以正常访问了
sudo firewall-cmd --zone=public --add-port=2375/tcp --permanent
sudo firewall-cmd --reload
```

* 日志相关配置
  * --log-level
    * daemon一般运行在后台,只能通过日志方式查看运行状态
    * 默认日志等级为info, 可设置值:debug info warn error fatal
  * --log-driver
    * 设置容器日志记录方式,默认为json-file
    * 其他有syslog journald fluentd gelf awslogs none
    * 要使用docker logs命令查看容器日志就必须设置为json-file
  * --log-opt
    * 设置日志记录方式的参数
    * 如max-size(超过该阀值的日志会回滚) max-file(回滚日志的最大文件数,超过数量的日志文件会被丢弃,没有设置max-size则不生效)等

* 存储相关配置
  * --graph
    * 设置docker运行的根目录,镜像和容器都会保存在该目录下
  * --storage-driver
    * docker启动容器时会创建文件系统,为rootfs提供挂载点
    * 支持多种文件系统 AUFS Devicemapper ZFS OverlayFS等
    * AUFS docker最早支持的文件系统,但未能加入linux内核,可能引起内核崩溃,但它是唯一支持在容器间共享程序和库的文件系统
    * Devicemapper
  * --storage-opt
    * 不同镜像存储方式有不同的参数,通过--storage-opt进行设置
    * 在设置Devicemapper相关参数时要加dm前缀,ZFS相关参数则需要添加zdf前缀

* 网桥相关配置
  * docker安装完成后会建立docker0虚拟网桥用于容器和外界通信
  * 默认情况下docker会自动配置docker0的ip子网掩码和容器范围
  * --bip
    * 设置docker0的ip和子网掩码
    * 如 --bip=172.0.0.1/24
  * --fixed-cidr
    * 设置容器ip的范围,创建容器时,docker daemon会从--fixed-cidr配置的ip范围内给容器的eth0网卡分配ip,再将docker0的子网掩码配置给eth0
  * --default-gateway
    * 默认情况下,容器使用docker0的ip作为网关
    * 可以通过--default-gateway设置新的容器网关
  * --dns
    * 配置dns地址

* 容器与外部通信配置
  * 容器要在宿主机配置两个参数才能和外部进行通信
    * ip_forward设置为1 允许宿主机向外转发
    * iptables规则运行向外连接
    * 设置 --ip_forward为true时,docker daemon启动会自动修改宿主机的ip_forward为1
    * 设置 --iptables为true时,docker daemon启动时会在iptables内追加转发规则,默认都为true
