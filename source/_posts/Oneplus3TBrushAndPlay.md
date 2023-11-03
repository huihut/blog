---

title: 一加3T的刷机流程及玩机一些事
subtitle: 一加3T的刷机流程及玩机一些事
date: 2017-08-27 13:54:57
author: huihut
tags:
	- 刷机
categories: 
	- CS
	
---

## 前言

本文主要聊聊一加3T卡刷第三方Rom，包括刷 TWRP 的 Recovery，卡刷流程，初始化，安装及使用Xposed框架和Magisk框架。

## 选择Rom

刷机前要选择要刷的系统，即选择Rom。

Rom无非官方Rom或者第三方Rom。

官方Rom有氢OS(H2OS)、氧OS(OxygenOS)。第三方Rom就很多了，如MIUI、Flyme、CM、AICP、LineageOS等等。

官方Rom一般比较稳定、适配性好、能接收推送；第三方Rom一般自定义程度高、可玩性高。

若是不想折腾还是官方Rom好；喜欢尝鲜可以试试第三方Rom。

### 官方Rom

一加官方Rom有氢OS(H2OS)和氧OS(OxygenOS)。氢OS(H2OS)主要面向国内，本土化定制；氧OS(OxygenOS)主要面向国外，预装谷歌服务。官方Rom无论线刷还是卡刷都比较简单方便，在此就不做过多介绍。

<!-- more -->

#### 氢OS(H2OS)

下载：[H2OS官方下载](http://www.h2os.com/download)

#### 氧OS(OxygenOS)

下载：[OxygenOS官方下载](http://downloads.oneplus.net/)

### 第三方Rom

本文主要聊聊一加3T卡刷第三方Rom。

第三方Rom在各大玩机论坛/Rom平台都能看到，如

* 官方论坛（推荐）
* xda-developers（推荐）
* 机锋Rom
* ROM之家
* ...

如一加3T的

* [一加社区 . 一加手机3T](http://www.oneplusbbs.com/forum-116-1.html)
* [xda-developers . OnePlus 3T ROMs](https://forum.xda-developers.com/oneplus-3t/development#romList)

我选了个`OnePixel`，这是个基于氧OS的类Pixel版Rom，喜欢原生的朋友可以给你的爱机食用：

[[ROM][EAS][DEODEXED][OB13] OnePixel, OOS Based Custom Rom with Pixel Experience](https://forum.xda-developers.com/oneplus-3t/development/onepixel-oos-based-custom-rom-pixel-t3650386)

效果图


![OnePixel](http://huihut-img.oss-cn-shenzhen.aliyuncs.com/OnePixel.png)

## 安装 TWRP

因为一加官方 Recovery 功能较弱，所以先刷个 TWRP Recovery。

### 下载 TWRP

选择自己机型的TWRP下载

[https://twrp.me/Devices/](https://twrp.me/Devices/)

如我的一加3T下载的TWRP文件为：

    twrp-3.1.1-2-oneplus3t.img

### 安装 ADB

安装 ADB 可以使用一些一键安装工具（如：15秒ADB安装器）或者下载官方 Android SDK 的 platform-tools

#### 启用USB调试

不管哪种方式都需要首先将手机连上电脑，启用USB调试。

设置 > 开发人员选项 > USB调试

#### 使用15秒ADB安装器安装

![15 seconds ADB Installer](http://huihut-img.oss-cn-shenzhen.aliyuncs.com/15secondsADBInstaller.png)

作者发的帖子: [[TOOL] [WINDOWS] ADB, Fastboot and Drivers - 15 seconds ADB Installer v1.4.3](https://forum.xda-developers.com/showthread.php?p=48915118#post48915118)

国内百度云盘下载：

链接：<http://pan.baidu.com/s/1c1FWmTM> 密码：vg0g

#### 使用 Android SDK 的 platform-tools 安装

##### 下载 platform-tools

###### Windows版

* 官方下载：<https://dl.google.com/android/repository/platform-tools-latest-windows.zip>
* 网盘下载链接：<http://pan.baidu.com/s/1eRG8gXs> 密码：72ac

###### MacOS版

* 官方下载：<https://dl.google.com/android/repository/platform-tools-latest-darwin.zip>
* 网盘下载链接：<http://pan.baidu.com/s/1pLQFvrt> 密码：7qav

###### Linux版

* 官方下载：<https://dl.google.com/android/repository/platform-tools-latest-linux.zip>
* 网盘下载链接：<http://pan.baidu.com/s/1nu8mvrF> 密码：giug

##### 安装

解压，进入 platform-tools 目录

把`twrp-3.1.1-2-oneplus3t.img`文件复制到`platform-tools`目录下，终端使用

    adb reboot bootloader

进入bootloader模式，输入命令

    fastboot flash recovery twrp-3.1.1-2-oneplus3t.img

进行安装TWRP。

## 刷机（卡刷）

### Wipe - 清理系统和缓存

进入TWRP后，首先Wipe

进入Wipe - Advanced Wipe

勾选

* Dalvik/ART Cache
* Cache
* Data
* System

划过 Swipe to Wipe 来 Wipe

注：下面两个不用勾选

* Internal Storage 是数据存放的地方
* USB-OTG 是与支持USB-OTG设备的连接

### Install - 安装Rom

连上电脑在TWRP模式下，把下载的Rom拷贝到Internal Storage，然后在`Install`里面找到你的Rom，如

    OnePixel_OB13_oneplus3t-7.1.1rc.zip

划过`Swipe to confirm Flash`进行安装，装好后重启进入系统。

## 初始化设置

进入系统后进行初始化设置

![OnePixelInit](http://huihut-img.oss-cn-shenzhen.aliyuncs.com/OnePixelInit.png)

注意：WiFi/数据连接没代理不要连接，断网设置，不然会一直连接不上Google而卡在那里的。

初始化好之后设置了些东西就是这样子啦

![](http://huihut-img.oss-cn-shenzhen.aliyuncs.com/OnePixelInstalled.jpg)

## Xposed框架

大名鼎鼎的Xposed框架，可以通过添加各种模块实现各种功能，不过官方版现在才更新到适配Android6.0

官方链接：[Xposed Installer](http://repo.xposed.info/module/de.robv.android.xposed.installer)

好在XDA社区有 Material Design 版的Xposed框架下载器（推荐）

[xda-developers . Material Design Xposed Installer](https://forum.xda-developers.com/devdb/project/?id=13191#downloads)

安装 Material Design Xposed Installer 后在 UnOfficial 这栏选择适合自己手机型号的Xposed框架下载

![XposedUnOfficial](http://huihut-img.oss-cn-shenzhen.aliyuncs.com/XposedUnOfficial.jpg)

装好Xposed框架后，下载Xposed模块

![XposedModuleDownload](http://huihut-img.oss-cn-shenzhen.aliyuncs.com/XposedModuleDownload.jpg)

知乎上有些好的Xposed模块推荐

[知乎 . Android 系统上的 Xposed 框架中都有哪些值得推荐的模块？](https://www.zhihu.com/question/22063862/answer/95339323)

## Magisk框架

Magisk是类似xposed的第三方应用接口，但是暂时模块较少。

Magisk的介绍、下载、安装

[[2017.7.20] Magisk v13.3 - Root & Universal Systemless Interface [Android 5.0+]](https://forum.xda-developers.com/apps/magisk/official-magisk-v7-universal-systemless-t3473445)

下面是安利Magisk的帖子，里面有各种模块介绍：

[[教程] (搬运XDA)magisk框架教程以及模块-xposed的替代品](http://www.oneplusbbs.com/thread-3302341-1-1.html)

我因为OnePixel Rom自带Magisk，也就顺便装了个`Pixel Launcher transparent dock`，让pixel luncher底栏的颜色变透明。

![PixelLauncherTransparentDock](http://huihut-img.oss-cn-shenzhen.aliyuncs.com/PixelLauncherTransparentDock.jpg)

效果如上面初始化设置那里的截图。
