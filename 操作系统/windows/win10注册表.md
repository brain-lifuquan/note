# win10注册表

## 去掉此电脑下的几个文件夹

* 对应注册表项

```regedit
# 此电脑的文件夹目录 和侧边栏都管
计算机\HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\FolderDescriptions
# 另存为界面下侧边栏
计算机\HKEY_LOCAL_MACHINE\SOFTWARE\WOW6432Node\Microsoft\Windows\CurrentVersion\Explorer\FolderDescriptions\
# 修改 PropertyBag 下 ThisPCPolicy 值为 Hide Show表示显示
# 如果不存在 PropertyBag 可以新建项， 并在项下添加字符串属性 ThisPCPolicy 并设置属性值为 Hide
# 修改完成后在任务管理器中重新启动  Windows资源管理器 即可生效
# 3D对象因为是新建的项,必须重启电脑才能生效
```

* 文件夹对应关系：

```regeidt
{0ddd015d-b06c-45d5-8c4c-f59713854639}  图片
{31C0DD25-9439-4F12-BF41-7FF4EDA38722}  3D对象 需要创建PropertyBag项
{35286a68-3c57-41a1-bbb1-0eae73d76c95}  视频
{7d83ee9b-2244-4e70-b1f5-5393042af1e4}  下载
{a0c69a99-21c8-4671-8703-7934162fcf1d}  音乐
{B4BFCC3A-DB2C-424C-B029-7FE99A87C641}  桌面
{f42ee2d3-909f-4907-8871-4c22fc0bf756}  文档
```

* 控制面板卸载软件列表

```regedit
# 32位
计算机\HKEY_LOCAL_MACHINE\SOFTWARE\WOW6432Node\Microsoft\Windows\CurrentVersion\Uninstall
# 64位
计算机\HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Uninstall
```

* 修改CMD样式

```regedit
计算机\HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Command Processor
# DefaultColor 代表颜色
# 两个16进制位 前面1位代表背景色，后面1位代表前景色
# 如 0a 代表背景黑色，前景淡绿色
```

* 修改系统字体

```regedit
HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\WindowsNT\CurrentVersion\Fonts
```

* 远程桌面连接的IP记录

```regedit
计算机\HKEY_CURRENT_USER\Software\Microsoft\Terminal Server Client\Default
```

* 资源管理器侧边栏的OneDrive

```regedit
计算机\HKEY_CURRENT_USER\Software\Classes\CLSID\{018D5C66-4533-4307-9B53-224DE2ED1FE6}
# System.IsPinnedToNameSpaceTree
# 修改 System.IsPinnedToNameSpaceTree 值 从1 修改为 0
# 即时生效
```

* 右键菜单-授予访问权限

```regedit
计算机\HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Shell Extensions
新建/项 Blocked
新建/字符串值    {F81E9010-6EA4-11CE-A7FF-00AA003CA9F6}
```

* 右键菜单-兼容性疑难解答

```regedit
计算机\HKEY_CLASSES_ROOT\exefile\shellex\ContextMenuHandlers\Compatibility
```

* 右键菜单-包含到库中

```regedit
计算机\HKEY_CLASSES_ROOT\Folder\shellex\ContextMenuHandlers\Library Location
删除 Library Location
```

* 显卡右键菜单

```regedit
计算机\HKEY_CLASSES_ROOT\Directory\Background\shellex\ContextMenuHandlers
“igfxcui”键正是显卡的右键菜单项目
```

* 去掉文档浏览器左侧快速访问菜单

```regedit
\HKEY_CLASSES_ROOT\CLSID\{679f85cb-0220-4080-b29b-5540cc05aab6}\ShellFolder
先在ShellFolder右键修改权限
权限--高级--所有者（默认是SYSTEM）更改--Everyone--确定
上一页里找到 自己的用户（adminstrator） 允许完全控制 确定
Attributes 属性值 由a0100000 修改为 a0600000
然后权限改回去
重启电脑
```

* 右键菜单在vs中打开

```regedit
.reg脚本
Windows Registry Editor Version 5.00
[-HKEY_CLASSES_ROOT\Directory\Background\shell\AnyCode]
[-HKEY_CLASSES_ROOT\Directory\shell\AnyCode]
```

* 文件夹右键菜单

```regedit
计算机\HKEY_CLASSES_ROOT\Directory\background
```
