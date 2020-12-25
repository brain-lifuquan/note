# linux 远程连接

> Linux上的远程桌面连接主要使用两种协议
> 一种是微软在windows上的RDP(Remote Desktop Protocol)协议
> 另一种是VNC(Virtual Network Console)协议

## RDP协议

* rdesktop 实现了RDP协议
  * windows上一定要去掉勾选 仅允许运行使用网络级别身份验证的远程桌面的计算机连接

```sh
# 安装rdesktop
sudo dnf install rdesktop
# 连接到windows
rdesktop -g 1440x900 -r sound:off -u administrator -p lfq 192.168.22.153:3389
# 参数 意义
# -g 后面代表要使用的分辨率
# -P 启用位图缓存
# -z 启用RDP数据流压缩
# -x l 使用局域网级别的图像质量
# -r sound:off 关闭声音
# -u windowuser 指定要使用的用户
# IP地址 要连接的计算机的IP地址和端口号
```

* xrdp
  * 一个RDP服务端，可以让我们用远程桌面方式登录到Linux系统

## vnc

> vnc(virtual Network Console 虚拟网络控制台) 一款优秀的远程控制工具软件,由AT&T欧洲研究实验室开发,是在基于UNIX和Linux操作系统的免费的开源软件<br>
> <hr>
> 包括4个命令
>
> * vncserver
> * vncviewer
> * vncpasswd
> * vncconnect

```sh
# vncserver安装
sudo yum install tigervnc tigervnc-server -y
# 启动和关闭vncserver
# 启动vncserver 端口号5901
vncserver :1
# 检查启动情况 查看端口号
netstat -lntp | grep 5901
# 关闭
vncserver -kill :1
# 配置vnc登录密码(和系统密码无关)
vncpasswd
Password:
Verify:
# 还会提示设置一个只读密码,可设可不设
# 配置防火墙
sudo firewall-cmd --permanent --add-service vnc-server
sudo systemctl restart firewalld
# 在window上可以通过vncviewer客户端输入密码进入桌面
```
