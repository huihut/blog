---

title: 技嘉Z370 HD3P + i7-8700K + GTX1080 装黑苹果 High Sierra 10.13.6
subtitle: 技嘉Z370 HD3P + i7-8700K + GTX1080 装黑苹果 High Sierra 10.13.6
date: 2018-10-13 13:00:00
author: huihut
tags:
	- Mac
categories: 
	- CS
	
---

## 前言

本博文记录了组装台式机装黑苹果 High Sierra 10.13.6 的经历。

原本想装 Mojave 10.14 的，可惜发现在 Mojave 下还没有 GTX1080 的驱动，所以只能退而求其次装 High Sierra 了。

装 High Sierra 的过程中，第一次使用 10.13.6(17G2112) 镜像遇到个问题（下文有描述），无法进入安装界面，因此后来使用 10.13.5(17F77) 镜像装好后在 AppStore 更新 10.13.6

<!-- more -->

![Hackintosh-High-Sierra-10.13.6](http://huihut-img.oss-cn-shenzhen.aliyuncs.com/Hackintosh-High-Sierra-10.13.6.jpeg)

## 配置

* 主板：技嘉 Z370 HD3P
* CPU：Intel Core i7-8700K
* 核显：Intel UHD Graphics 630
* 独显：七彩虹 iGame GTX1080 Vulcan X 8G
* 内存：海盗船复仇者 DDR4 3200 8G*2
* 固态硬盘：三星 SSD 970 EVO 250GB（Windows）
* 机械硬盘：西数 WD10EZEX 7200PPM 64M 1T（EFI + MacOS + Storage）
* 板载音频：Realtek ALC1220
* 板载网卡：Intel I219V2 PCI Express Gigabit Ethernet
* USB蓝牙：绿联 CSR8891 USB蓝牙 4.0

## 文件

本博文使用的一些软件工具驱动，下文则不赘述。

* [带 Clover 镜像](https://mirrors.dtops.cc/iso/MacOS/daliansky_macos/)
    * 10.13.6(17G2112) 镜像：[macOS High Sierra 10.13.6(17G2112) Installer with Clover 4606.dmg](https://mirrors.dtops.cc/iso/MacOS/daliansky_macos/macOS%20High%20Sierra%2010.13.6%2817G2112%29%20Installer%20with%20Clover%204606.dmg)
    * 10.13.5(17F77)镜像：[macOS High Sierra 10.13.5(17F77) Installer with Clover 4512.dmg](https://mirrors.dtops.cc/iso/MacOS/daliansky_macos/macOS%20High%20Sierra%2010.13.5%2817F77%29%20Installer%20with%20Clover%204512.dmg)
* EFI 分区
    * [huihut . Hackintosh-Z370/EFI](https://github.com/huihut/Hackintosh-Z370/tree/master/EFI)
* 工具
    * U盘刻录工具 Etcher：[官网](https://etcher.io/) | [百度网盘](https://pan.baidu.com/s/1EAarG7mLxhI0xwQEXerUEg)
    * Clover 配置工具 Clover Configurator：[官网](https://www.tonymacx86.com/resources/clover-configurator.335/) | [百度网盘](https://pan.baidu.com/s/1adKnfyIT0MVwHGl_Lke_Fg)
    * 内核注入工具 Kext Wizard：[网络](https://mac.softpedia.com/get/Utilities/Kext-Wizard.shtml) | [百度网盘](https://pan.baidu.com/s/1bnsRR4s3FYmc6jCRiVYRew)
    * 文本编辑器 BBEdit 12：[官网](https://www.barebones.com/products/bbedit/) | [百度网盘](https://pan.baidu.com/s/1pWO_hFMcHIjzoFGSOdUgDQ)
* 驱动
    * 驱动安装工具 MultiBeast 10.4.0 - High Sierra：[官网](https://www.tonymacx86.com/resources/multibeast-10-4-0-high-sierra.401/) | [百度网盘](https://pan.baidu.com/s/1QBX514ELiqltAJ_EcHan0g)
    * Nvidia Web 驱动 387.10.10.10.40.105（适用于 10.13.6）：[官网](https://www.nvidia.com/download/driverResults.aspx/136062/en-us) | [百度网盘](https://pan.baidu.com/s/1pgf06gmxiwpK1256_7QmcQ)
    * Nvidia Web 驱动 387.10.10.10.35.106（适用于 10.13.5）：[官网](https://www.nvidia.com/download/driverResults.aspx/134834/en-us) | [百度网盘](https://pan.baidu.com/s/13ObVcKgRqr1XIvDv2d96Eg)

## 刻录镜像

准备一个8G以上的U盘，使用 Etcher 刻录上面下载的镜像（带有 Clover EFI 分区）：`macOS High Sierra 10.13.6(17G2112) Installer with Clover 4606.dmg`

![etcher-create-hackintosh](http://huihut-img.oss-cn-shenzhen.aliyuncs.com/etcher-create-hackintosh.png)

## 设置 BIOS

> BIOS 版本已更新为 F7

* Save & Exit -> Load Optimized Defaults : Yes
* M.I.T. -> Advanced Memory Settings -> Extreme Memory Profile(X.M.P.) : Profile 1
* BIOS -> Fast Boot : Disabled
* BIOS -> CSM Support : Disabled
* Peripherals -> Super IO Configuration -> Serial Port : Disabled
* Peripherals -> USB Configuration -> XHCI Hand-off : Enabled
* Chipset -> Vt-d : Disabled

保存 BIOS 配置

## 引导出错

重启进入刻录好的U盘，选择 `Boot macOS Install from Install macOS High Sierra`

![Clover](http://huihut-img.oss-cn-shenzhen.aliyuncs.com/IMG_20181007_204929.jpg)

唠叨模式滚代码的时候出错：please go to https://panic.apple.com to report this panic

![please go to https://panic.apple.com to report this panic](http://huihut-img.oss-cn-shenzhen.aliyuncs.com/IMG_20181007_211514.jpg)

## 重新刻录

上面的问题 Google 了一圈并未发现解决办法，因此重新刻录 `macOS High Sierra 10.13.5(17F77) Installer with Clover 4512.dmg`，先装 10.13.5(17F77) 。

## 引导安装

这次的 10.13.5(17F77)  成功进入安装界面了。

打开磁盘工具，格好一个系统盘，格式为 APFS 或者 Mac OS 扩展（日志式），大小因人而异，我留了 128G。

格好后安装系统到这个盘。

安装过程中第一次自动重启依然进入 U 盘 Clover，引导进入格出来的盘继续安装。

第二次自动重启也是进入 U 盘 Clover，引导进入格出来的盘，然后安装完毕进入系统。

## 安装 Clover

### Clover 选择

进入系统后发现 1080 独显不能驱动、八代 CPU 不能识别，这个先不管，先装 Clover 到磁盘的 EFI 分区。

安装 Clover 有两种：

* 使用原版 CLover：[Clover EFI bootloader 官方下载](https://sourceforge.net/projects/cloverefiboot/)
* 使用 U 盘 EFI 分区的 Clover

由于 U 盘的 Clover 是已经试验过能引导的，所以我就用了 U 盘的。

### 挂载 EFI 分区

在终端上执行 `diskutil list`，找到两个 EFI 分区（磁盘的 EFI 和 U 盘的 EFI），如下的 `disk0 s1` 和 `disk3 s1`

```
HuiMac:~ huihut$ diskutil list
/dev/disk0 (internal, physical):
   #:                       TYPE NAME                    SIZE       IDENTIFIER
   0:      GUID_partition_scheme                        *1.0 TB     disk0
   1:                        EFI EFI                     209.7 MB   disk0s1
   2:                 Apple_APFS Container disk2         128.8 GB   disk0s2
   3:       Microsoft Basic Data Document                85.9 GB    disk0s3
   4:       Microsoft Basic Data Software                161.1 GB   disk0s4
   5:       Microsoft Basic Data Data                    624.2 GB   disk0s5

/dev/disk1 (internal):
   #:                       TYPE NAME                    SIZE       IDENTIFIER
   0:      GUID_partition_scheme                         250.1 GB   disk1
   1:         Microsoft Reserved                         16.8 MB    disk1s1
   2:       Microsoft Basic Data                         249.2 GB   disk1s2
   3:           Windows Recovery                         847.2 MB   disk1s3

/dev/disk2 (synthesized):
   #:                       TYPE NAME                    SIZE       IDENTIFIER
   0:      APFS Container Scheme -                      +128.8 GB   disk2
                                 Physical Store disk0s2
   1:                APFS Volume MacOS                   84.8 GB    disk2s1
   2:                APFS Volume Preboot                 67.7 MB    disk2s2
   3:                APFS Volume Recovery                1.0 GB     disk2s3
   4:                APFS Volume VM                      20.5 KB    disk2s4

/dev/disk3 (external, physical):
   #:                       TYPE NAME                    SIZE       IDENTIFIER
   0:      GUID_partition_scheme                        *31.0 GB    disk3
   1:                        EFI EFI                     209.7 MB   disk3s1
   2:                  Apple_HFS Install macOS High S... 5.7 GB     disk3s2

HuiMac:~ huihut$ 
```

分别挂载到不同名字的卷

挂载磁盘 EFI
```
sudo mkdir /Volumes/efidisk
sudo mount -t msdos /dev/disk0s1 /Volumes/efidisk
```

挂载 U 盘 EFI
```
sudo mkdir /Volumes/efiusb
sudo mount -t msdos /dev/disk3s1 /Volumes/efiusb
```

### 复制 Clover

然后把 U 盘的 `EFI/CLOVER` 这个文件夹复制到 磁盘的 `EFI` 下 

## 安装驱动

使用 MultiBeast 安装驱动，使用 Nvidia WebDriver 安装显卡驱动（MultiBeast 的 WebDriver 不能驱动我的 1080）。

![MultiBeast-config](http://huihut-img.oss-cn-shenzhen.aliyuncs.com/Jietu20181007-221705.jpg)

装好 Nvidia WebDriver 后 1080 能驱动，不过 i7-8700K 的八代 CPU 还是不能识别，先不管，在 AppStore 更新 10.13.6。

## 更新后安装驱动

更新完 10.13.6 发现许多驱动有问题：

* 独显无法驱动
* CPU无法识别
* 声卡无法驱动
* USB3.0无法驱动

### 驱动独显

使用适用于 10.13.6 的 WebDriver-387.10.10.10.40.105.pkg，装好重启后就 OK 了

![](http://huihut-img.oss-cn-shenzhen.aliyuncs.com/Hackintosh-gpu.jpg)

### 修复 CPU 识别

> 参见 [八代处理器安装黑苹果 关于本机显示“未知”解决办法](https://osx.cx/8-dai-cpu-installed-black-apple.html)

挂载磁盘 EFI 分区（操作如上），使用 Clover Configurator 打开 `/EFI/CLOVER/config.plist`

在 CPU 页面的 Type 中填入 Unknown，保存。

![](http://huihut-img.oss-cn-shenzhen.aliyuncs.com/Jietu20181007-232553.jpg)

使用文本编辑器（如 BBEdit）打开 `/System/Library/PrivateFrameworks/AppleSystemInfo.framework/Versions/A/Resources/zh_CN.lproj/AppleSystemInfo.strings` 文件

修改 `UnknownCPUKind` 的值为 `3.7 GHz Intel Core i7-8700K`

![3.7 GHz Intel Core i7-8700K](http://huihut-img.oss-cn-shenzhen.aliyuncs.com/Jietu20181007-233544.jpg)

保存重启即可。

![](http://huihut-img.oss-cn-shenzhen.aliyuncs.com/Hackintosh-system-information.jpg)

### 修复声卡驱动

> 参见 [黑苹果AppleALC声卡驱动安装使用教程](https://imac.hk/applealc-kext-use.html)

我使用 MultiBeast 安装的 ALC1220 驱动在 10.13.6 下不能驱动我的声卡，试过 MultiBeast 的其他驱动也不行，因此使用另一种方法修复。

在 [acidanthera/AppleALC/releases](https://github.com/acidanthera/AppleALC/releases) 下载最新的 AppleALC 的 RELEASE 版 `AppleALC.kext`，使用 Kext Wizard 注入这个内核，如下图

![Kext Wizard AppleALC.kext](http://huihut-img.oss-cn-shenzhen.aliyuncs.com/Jietu20181007-225717.jpg)
![Kext Wizard AppleALC.kext](http://huihut-img.oss-cn-shenzhen.aliyuncs.com/Jietu20181007-225733.jpg)

然后挂载磁盘 EFI 分区（操作如上），把 `AppleALC.kext` 放入磁盘 EFI 分区的 `EFI/CLOVER/kexts/Other/`目录中。

然后在 [acidanthera/AppleALC/Resources](https://github.com/acidanthera/AppleALC/tree/master/Resources) 找到你的声卡型号的文件夹，进入（如我的是 `ALC1220`）。

![ALC1220](http://huihut-img.oss-cn-shenzhen.aliyuncs.com/Jietu20181007-230207.jpg)

我的声卡型号看到有 `layout1.xml`、`layout2.xml`、`layout5.xml`、`layout7.xml`、`layout11.xml`、`layout13.xml`

然后在 1、2、5、7、11、13 中随便取一个数。

使用文本编辑器打开磁盘 EFI 分区的 `EFI/CLOVER/config.plist` 文件，搜索 `<key>Audio</key>`，把它的 `integer` 改为刚刚取的那个数（如 `1`）。

![Audio](http://huihut-img.oss-cn-shenzhen.aliyuncs.com/Jietu20181007-231402.jpg)

保存重启即可。

![](http://huihut-img.oss-cn-shenzhen.aliyuncs.com/Hackintosh-audio.jpg)

### 修复 USB3.0

> 参见 [HACKINTOSH HIGH SIERRA 10.13.6 UPDATE GUIDE](https://hackintosher.com/guides/hackintosh-high-sierra-10-13-6-update-guide/)

使用上文描述的操作挂载磁盘的 EFI 分区

使用 Clover Configurator 打开 `/EFI/CLOVER/config.plist`

如下图，插入（若已存在则修改为）配置信息

* Name*: com.apple.driver.usb.AppleUSBXHCI
* Find* [HEX]: 837D880F 0F83A704 0000
* Replace* [HEX]: 837D880F 90909090 9090
* Comment: USB 10.13.6+ by PMHeart
* MatchOS: 10.13.x

![Clover Configurator USB3.0](http://huihut-img.oss-cn-shenzhen.aliyuncs.com/Jietu20181007-223654.jpg)

保存重启即可。

![](http://huihut-img.oss-cn-shenzhen.aliyuncs.com/hackintosh-usb3.jpg)

### 蓝牙免驱

![](http://huihut-img.oss-cn-shenzhen.aliyuncs.com/Hackintosh-bluetooth.jpg)

### iMessage、序列号问题

> 参见 [[Hackintosh]解决黑苹果无法使用Siri、iMessage等服务](http://www.ituring.com.cn/article/274156)

## 更换Clover主题

Clover主题可以到官方仓库下载：

* [Clover Theme Repository](https://clover-wiki.zetam.org/theme-database)

也可以使用其他的，比如我使用这个简洁风格的：

* [Clover Minimal - A clean theme for the Clover UEFI bootloader](https://github.com/al3xtjames/clover-theme-minimal)

![clover-theme-minimal](https://camo.githubusercontent.com/df4adb0f442a1baf2ec59cf17f3442253619af6f/687474703a2f2f692e696d67626f782e636f6d2f346773734c6453492e706e67)

只需下载下来，放到 `/EFI/CLOVER/themes` 文件夹下，然后使用 Clover Configurator 打开 `/EFI/CLOVER/config.plist` 更换到这个主题就好啦。

![更换Clover主题](http://huihut-img.oss-cn-shenzhen.aliyuncs.com/Jietu20181013-001702.jpg)

另外这个显示启动盘的标签的 `Label` 建议勾上，这样才知道选择的是哪个盘，保存重启即可。

## 感谢

* [tonymacx86](https://www.tonymacx86.com/)
* [黑果小兵的部落阁](https://blog.daliansky.net/)
* [黑苹果乐园](https://imac.hk/)
* [[Success] Gigabyte Z370 HD3P - i7 8700K - Gigabyte RX580 4GB - Mojave 10.14.0](https://www.tonymacx86.com/threads/success-gigabyte-z370-hd3p-i7-8700k-gigabyte-rx580-4gb-mojave-10-14-0.256221/)
* [Building a GTX 1080 Ti-powered Hackintosh: Installing macOS Sierra step-by-step [Video]](https://9to5mac.com/2017/04/28/building-a-gtx-1080-ti-powered-hackintosh-installing-macos-sierra-step-by-step-video/)