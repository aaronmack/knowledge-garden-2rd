---
{"dg-publish":true,"permalink":"/adv/os-windows/","title":"OS - Windows","noteIcon":""}
---


这里打算长久更新这篇，去记录与OS-Windows相关的东西

## Windows激活

发现一个非常方便的Windows激活工具，但可能需要科学上网才行~

[https://github.com/massgravel/Microsoft-Activation-Scripts](https://github.com/massgravel/Microsoft-Activation-Scripts)
https://github.com/TGSAN/CMWTAT_Digital_Edition/releases/tag/2.6.4.0

## Windows优化

这个工具下载下来后可以直接运行，里面有非常多的选项，比如开启关闭Windows更新，禁用一些缓存等等~

[https://github.com/hellzerg/optimizer](https://github.com/hellzerg/optimizer)

## Windows 禁用Defender

忘记了是用哪一个去关闭的Windows的Defender，感觉其实只要不下载来路不明的app一般都不需要这个，或者除非黑客盯上你了，就是要攻击你~

[https://github.com/ionuttbara/windows-defender-remover](https://github.com/ionuttbara/windows-defender-remover)

[https://github.com/swagkarna/Defeat-Defender-V1.2.0](https://github.com/swagkarna/Defeat-Defender-V1.2.0)

https://github.com/qtkite/defender-control

> [!WARNING] （windows-defender-remover）在移除时一定要选择Safe的选项，这样可以Rollback。但不知为何，会使默认的windows-cmd变成管理员权限的，这样导致一些软件不能正常使用。
## Windows 磁盘映射

有时候你有个WebDAV或者OneDrive，但是Windows自带的磁盘映射似乎很容易卡死，在查找对应的替代方案是，找到了RaiDrive，可以映射很多很多种服务~

[https://www.raidrive.com/](https://www.raidrive.com/)

## Windows 磁盘检测工具

**SpaceSniffer** 可以分析磁盘上的文件大小占用空间情况~

**CrystalDiskMark** 磁盘读写速度测试~

**CrystalDiskInfo** 查看磁盘健康情况~


## Windows下的sudo与服务管理

今天在测试alist的windows本地部署时，发现了nssm这个工具（在alist的官方文档中提到的[Manual installation | AList Docs](https://alist.nn.ci/guide/install/manual.html#get-alist)），我使用的是scoop去管理win上面的程序安装和卸载

scoop install nssm
scoop install sudo

还有一个gsudo，不知这两个有何区别。

## Windows下的开发工具

开发人员的瑞士军刀 [GitHub - veler/DevToys: A Swiss Army knife for developers.](https://github.com/veler/DevToys)

scoop install devtoys-np

## Windows 美化Shell

oh-my-posh是适用于任何外壳的提示主题引擎。

1.安装clink 使用 `scoop install clink` 执行`clink autorun install`,这样默认启动cmd就是clink的界面了。

2.执行clink info, 找到scripts目录，进入目录后新建一个`oh-my-posh.lua`文件,内容如下

```lua
load(io.popen('oh-my-posh init cmd'):read("*a"))()
```
3.administrator打开cmd，执行`oh-my-posh font install`下载字体,需要开启代理。

4.安装方式 `scoop install https://github.com/JanDeDobbeleer/oh-my-posh/releases/latest/download/oh-my-posh.json`

## Windows 空间清理

[GitHub - SFYYH/cPanClear: C盘清理教程](https://github.com/SFYYH/cPanClear) 这个仓库中的README介绍了如何清理Windows的C盘去释放空间，这里面提到了Dism++这个软件，用它可以清理到一些更深层次的不需要的文件。

## Windows 关闭更新

[GitHub - WereDev/Wu10Man: Enable/Disable Windows 10 Automatic Updates](https://github.com/WereDev/Wu10Man)

这个项目的作者已经不维护了。┭┮﹏┭┮

## Visual Studio Extension - Viasfora

<img src="https://cdn.jsdelivr.net/gh/aaronmack/image-hosting@master/e/image.6j7x5r3wq840.png" alt="image" width="500"  />

一个非常好看的Code Style，比起之前只是一片蓝色要好很多。

## Windows 文件搜索

Everything - [voidtools](https://www.voidtools.com/)
Listary - [Listary – File Search & App Launcher](https://www.listary.com/)

自带的资源管理器中的资源搜索比较慢。可以使用everything。

## Windows 下文件解锁

有时候想要删除某个文件发现被占用，无法删除掉，这时候可以使用IObit Unlocker去解锁文件，这个软件安装完成后是个右键菜单。

[IObit Unlocker, Solution for "undelete files or folders" Problems on Windows 8, 7, Vista, XP, 10 - IObit](https://www.iobit.com/en/iobit-unlocker.php)


## Windows 下的代理

Proxifier - https://www.proxifier.com/download/

## Windows 鼠标键盘共享

1.可以以某台电脑为Server，其它电脑为Client，一套鼠标键盘控制多台电脑 barrier - [GitHub - debauchee/barrier: Open-source KVM software](https://github.com/debauchee/barrier)测试之后发现鼠标对不准，会在另外一边屏幕消失。Github上的也有提Issue。

2.Mouse without Borders
 下载网址  https://www.microsoft.com/en-us/download/details.aspx?id=35460 
 参考视频：[同时操控 2 台电脑，只需一个鼠标和键盘！完全免费，由微软官方提供 | 零度解说 - YouTube](https://www.youtube.com/watch?v=bcsZzhdrccs) 这个鼠标在某些地方上也会消失，比如开始菜单。

## Windows 开机启动

只需要把快捷方式放到 `%appdata%\Microsoft\Windows\Start Menu\Programs\Startup` 这个目录下，即可。

## Windows 更改文件夹所有者

`<directory> 右键 -> Properties -> Security -> Advanced -> Owner (Change)`

## Windows 下截图工具

1. Snipaste [Snipaste Downloads](https://www.snipaste.com/download.html)
2. FastStone Capture 

```bash
FSCapture 10.2:

username: Abuela Juana
Reg code: KYOQT-RXMFA-GVHKK-TAXPC
username: J3ud1/YouTube
Reg Code: GXFQN-RAMXF-RUBND-JNHGA
username: Mi mama Senayda
Reg Code: PPDNN-JEEGY-ARPNV-ESMXI

FSCapture 9.9:

username: Free Software 
Serial: BXRQE-RMMXB-QRFSZ-CVVOX
```

## Windows 安装Optional features

如果安装的Windows是Pro N版本，是不带Media Feature Pack的，这样在安装Camtasia录屏软件时会出错。
安装Media Feature Pack，在Windows设置 -> Apps -> Optional features -> Add a feature (搜索 Media Feature Pack)。

> [!NOTE] 如果不显示需要使用Administrator启动cmd，执行`sfc /scannow`如果这个命令执行失败，则可能是服务被关闭，需要执行`sc config trustedinstaller start= auto`再执行`net start trustedinstaller` , 再去执行sfc。参考[sfc /scannow "Windows Resource Protection could not start the repair service"](https://answers.microsoft.com/en-us/windows/forum/all/sfc-scannow-windows-resource-protection-could-not/3dabeaa4-269d-4c65-ab42-a3cf5ef1092d)


## Windows下比较两份(多份)文件(目录)不同

WinMegre。比较文件内容不同，比较目录不同。堪称神器。

## Windows 下任务栏统计数据显示

开源的小工具可以在任务栏显示当前网速，CPU使用率等等[Taskbar Stats is an open source tool that displays your computer's resource usage on the Windows Taskbar - gHacks Tech News](https://www.ghacks.net/2020/11/30/taskbar-stats-is-an-open-source-tool-that-displays-your-computers-resource-usage-on-the-windows-taskbar/)好像只支持Win10且已经不更新了。

## Windows 常用命令

```bash
## List All Shares
net share
 
## Stop Sharing a Folder
net share sharename /delete
```

# Windows下的常用软件代理

pip: `%APPDATA%\pip\pip.ini` 

```bash
[global]
proxy = http://user:password@proxy_name:port
```

git: `C:\Users\<username>\.gitconfig`

```bash
[http]
	proxy = http://127.0.0.1:10809
[https]	
	proxy = http://127.0.0.1:10809
```

npm:

```bash
# 设置代理
npm config set proxy=http://<server>:<port>
npm config set registry=http://registry.npmjs.org
npm config set https-proxy http://<server>:<port>

# 移除代理
npm config delete proxy
npm config delete https-proxy
```
# Windows CMD 中文显示

在CMD的顶部空白处右键，打开属性，选择字体，在字体选项栏中，将默认字体(我这里是`Consoles`)改为`KaiTi`，就可以显示中文了。

# Windows 设置系统级别代理

软件Proxifier可以做到程序级别，像Google浏览器插件Proxy SwitchySharp做到浏览器级别的代理。系统级别可以做到底层的代理，虽然不知道下方的设置是否正确，但起到了效果。

控制面板 -> Internet Options -> Connections -> Proxy server

<img src="https://cdn.jsdelivr.net/gh/aaronmack/image-hosting@master/e/image.7cx1k4pe9fgg.webp" alt="image" width=600/>