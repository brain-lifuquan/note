# ubuntu

## 软件安装

### ubuntu安装docker

```bash
# 卸载老版本
sudo apt-get remove docker docker-engine docker.io containerd runc
# 配置软件库
sudo apt-get update
sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg-agent \
    software-properties-common
# gpg key
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
sudo apt-key fingerprint 0EBFCD88
sudo add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable"
# 安装
sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io
# 将用户添加到docker组
sudo usermod -aG docker <your-user>
# 修改daemon
sudo vi /etc/docker/daemon.json
# 添加如下内容
{
  "registry-mirrors": ["https://hk8nhg9w.mirror.aliyuncs.com"]
}
# 保存退出
# 重新加载配置文件并重启docker
sudo systemctl daemon-reload
sudo systemctl restart docker
```

### zeal 离线api文档管理器

```bash
sudo apt-get install zeal
```

### unity-tweak-tool 界面优化工具

```bash
sudo apt install unity-tweak-tool
```

## 系统调整

> 一些预安装软件的卸载

```bash
#  卸载火狐浏览器
#  查找火狐详细内容：
dpkg --get-selections |grep firefox 
#  删除
sudo apt-get purge firefox* 
# 卸载文档查看器
# evince
sudo apt-get purge evince
sudo apt-get purge evince-common
# 卸载文本编辑器
# gedit
sudo apt-get purge gedit
sudo apt-get purge gedit-common
# 卸载软件市场
sudo apt autoremove --purge snapd
```

> 更换国内的软件管理源

* 查看ubuntu的Codename  这东西跟ubuntu的版本是对应的
  * 如果查不到可以根据自己的版本去搜一搜

```sh
# 查看ubuntu的Codename
root@DESKTOP-G6OQFJI:~# lsb_release -a | grep Codename | awk '{print $2}'
No LSB modules are available.
focal
# 备份系统源
mv /etc/apt/sources.list /etc/apt/sources.list.bk
# 写入阿里源
vi /etc/apt/sources.list
# 内容
deb http://mirrors.aliyun.com/ubuntu/ focal main multiverse restricted universe
deb http://mirrors.aliyun.com/ubuntu/ focal-backports main multiverse restricted universe
deb http://mirrors.aliyun.com/ubuntu/ focal-proposed main multiverse restricted universe
deb http://mirrors.aliyun.com/ubuntu/ focal-security main multiverse restricted universe
deb http://mirrors.aliyun.com/ubuntu/ focal-updates main multiverse restricted universe
deb-src http://mirrors.aliyun.com/ubuntu/ focal main multiverse restricted universe
deb-src http://mirrors.aliyun.com/ubuntu/ focal-backports main multiverse restricted universe
deb-src http://mirrors.aliyun.com/ubuntu/ focal-proposed main multiverse restricted universe
deb-src http://mirrors.aliyun.com/ubuntu/ focal-security main multiverse restricted universe
deb-src http://mirrors.aliyun.com/ubuntu/ focal-updates main multiverse restricted universe
# 执行更新
sudo apt-get update
```

<hr>

> 修改双系统 系统选择界面等待时间

```bash
# 使用gedit 打开配置文件
sudo gedit /etc/default/grub
# 修改配置 GRUB_TIMEOUT 由10 修改为2
GRUB_TIMEOUT=2
# 运行以下命令使修改生效
sudo update-grub
```

> 修改合盖子时自动关机

```bash
# 修改配置文件
sudo vi /etc/systemd/logind.conf
# HandleLidSwitch 对应的值改为 poweroff
```
