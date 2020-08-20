# 1. 安装Deepin-wine
本地安装(Ubuntu/Debian通用)
克隆 (git clone https://github.com/wszqkzqk/deepin-wine-ubuntu.git) 或下载到本地。

在中国推荐用下面的地址，速度更快： (git clone https://gitee.com/wszqkzqk/deepin-wine-for-ubuntu.git)

当然也可以选择下载releases：Github 或者 Gitee

解压后切换到解压文件目录，在终端中运行（授予可执行权限后）： `./install.sh`

KDE或其他按照普通安装方式安装后运行出现X错误的桌面环境执行 `./KDE-install.sh` ）。

本地安装deepin-wine的官方最新环境(目前2.18-22版本/仅ubuntu测试)
解压后切换到解压文件目录，在终端中运行（授予可执行权限后）： `./install_2.8.22.sh`


# 2.安装deepin-wine-wxwork
下载地址如下：（PS：我选的是deepin.com.wechat_2.6.8.65deepin0_i386.deb）
https://mirrors.aliyun.com/deepin/pool/non-free/d/deepin.com.wechat/

使用命令 `sudo dpkg -i deepin.com.wechat_2.6.8.65deepin0_i386.deb` 即可


# 3.解决乱码问题
1．查看应用是怎么运行的
```
cat /usr/share/applications/deepin.com.wechat.desktop
```

```
#!/usr/bin/env xdg-open
[Desktop Entry]
Encoding=UTF-8
Type=Application
X-Created-By=Deepin WINE Team
Categories=chat;
Icon=deepin.com.wechat
Exec="/opt/deepinwine/apps/Deepin-WeChat/run.sh" -u %u
Name=WeChat
Name[zh_CN]=微信
Comment=Tencent WeChat Client on Deepin Wine
StartupWMClass=WeChat.exe
MimeType=
```

２．查看运行脚本
```
cat /opt/deepinwine/apps/Deepin-WeChat/run.sh
```

```
#!/bin/sh
# Copyright (C) 2016 Deepin, Inc.
#
# Author: Li LongYu <lilongyu@linuxdeepin.com>
# Peng Hao <penghao@linuxdeepin.com>
BOTTLENAME="Deepin-WeChat"
APPVER="2.6.8.65deepin0"
EXEC_PATH="C:/Program Files/Tencent/WeChat/WeChat.exe"
if [ -n "$EXEC_PATH" ];then
/opt/deepinwine/tools/run_v2.sh $BOTTLENAME $APPVER "$EXEC_PATH" "$@"
else
/opt/deepinwine/tools/run_v2.sh $BOTTLENAME $APPVER "uninstaller.exe" "$@"
fi
```

３．修改对应脚本
```
sudo vim /opt/deepinwine/tools/run_v2.sh
```
```
# 将 WINE_CMD 那一行修改为
WINE_CMD="LC_ALL=zh_CN.UTF-8 deepin-wine"
```


# 4.修改字体(微软雅黑)
下载微软雅黑字体, [msyh.ttc](https://raw.githubusercontent.com/owent-utils/font/master/%E5%BE%AE%E8%BD%AF%E9%9B%85%E9%BB%91/MSYH.TTC)

1.添加字体 `cp msyh.ttc ~/.deepinwine/Deepin-WXWork/drive_c/windows/Fonts/`

2.修改系统注册表
```
gedit ~/.deepinwine/Deepin-WXWork/system.reg
```
```
#修改以下两行
"MS Shell Dlg"="msyh"
"MS Shell Dlg 2"="msyh"
```

3.字体注册
```
vim msyh_config.reg 
```
```
# 内容添加
REGEDIT4
[HKEY_LOCAL_MACHINE\Software\Microsoft\Windows NT\CurrentVersion\FontLink\SystemLink]
"Lucida Sans Unicode"="msyh.ttc"
"Microsoft Sans Serif"="msyh.ttc"
"MS Sans Serif"="msyh.ttc"
"Tahoma"="msyh.ttc"
"Tahoma Bold"="msyhbd.ttc"
"msyh"="msyh.ttc"
"Arial"="msyh.ttc"
"Arial Black"="msyh.ttc"
```
```
#注册
WINEPREFIX=~/.deepinwine/Deepin-WeChat deepin-wine regedit msyh_config.reg
```

4. 重新启动wxwork

# 5.解决WXWork无法发送图片问题
执行命令 `sudo apt install libjpeg62:i386`

# 6.升级wxwork至最新版

1.下载最新版企业微信 .exe文件

2.解压缩为文件夹

3.命令行中进入/home/自己用户名/.deepinwine/Deepin-WXWork/drive_c/Program Files/  

4.备份WXWork文件夹

5.然后将刚才解压的文件夹拖动进来改名为WXWork  重新打开企业微信即可


# 7.参考链接

https://github.com/wszqkzqk/deepin-wine-ubuntu

https://gist.github.com/koolay/bd4fc85da13d7047c262eafc7b4640ce

https://gitee.com/wszqkzqk/deepin-wine-for-ubuntu/issues/ILQCW

https://bbs.deepin.org/forum.php?mod=viewthread&tid=187933
