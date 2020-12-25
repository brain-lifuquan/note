# wsl

> Windows Subsystem for Linux<br>
> 适用于 Linux 的 Windows 子系统<br>
> <https://docs.microsoft.com/zh-cn/windows/wsl/><br>

## 常用命令

```powershell
wsl --set-default Ubuntu
wsl --set-default-version 2
wsl --set-version Ubuntu 2
```

## 安装 wsl1

* 启用适用于Linux的windows 子系统
* 以管理员身份打开PowerShell并运行
  * `dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart`
* 在windows商店找到发行版并安装
  * win10 1809 ltsc版本需要先安装windows商店（运行WindowsStore_LTSC2019）

* wsl-ubuntu20.04在win10中的位置
* `C:\Users\Administrator\AppData\Local\Packages\CanonicalGroupLimited.Ubuntu20.04onWindows_79rhkp1fndgsc\LocalState\rootfs`

## 配置伴随win10启动

* 在 `shell:startup` 下添加一个`vbs`脚本文件， 使用bat脚本也可以实现，但是开机时会打开一个`cmd`窗口，`vbs`可以后台运行

```vbs
Set ws = WScript.CreateObject("WScript.Shell")
cmd = "wsl -d Ubuntu -u root /etc/init.wsl"
ws.Run cmd, 0, false
Set ws = Nothing
WScript.quit
```

* 在`wsl`内部创建一个脚本文件`/etc/init.wsl`（文件名可以自定义，与`vbs`脚本中对应），写入要启动的服务
  * 修改 init.wsl 为可执行

```sh
vi /etc/init.wsl
#! /bin/sh
# 启动ssh
/etc/init.d/ssh start
# 修改 init.wsl 为可执行
sudo chmod +x /etc/init.wsl
```

* 重启电脑，已经可以自启动了，wsl2由于是虚拟机形式，启动比wsl要慢几秒钟
