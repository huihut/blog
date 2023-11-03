---

title: Flutter beta 版尝鲜（在 Windows + Android Studio 与 MacOS + VS Code 的安装配置）
subtitle: Flutter beta 版尝鲜（在 Windows + Android Studio 与 MacOS + VS Code 的安装配置）
date: 2018-03-13 19:06:43
author: huihut
tags:
- Flutter
categories: 
- CS

---


Flutter 是一个 Google 发布的跨平台移动 UI 框架，使用 Dart 语言开发，可以构建高质量原生 iOS 、Android 以及 Fuchsia OS 应用，并且在排版、图标、滚动、点击等方面实现零差异。

[Flutter 官网](https://flutter.io/)

鉴于最近出了 beta 版，就来尝鲜一下吧。

本文有 Windows + Android Studio 与 MacOS + VS Code 的体验。

<!-- more -->

## Windows + Android Studio 

### 获取 Flutter SDK

使用 [git](https://git-scm.com/) 克隆下来 Flutter SDK

```
git clone -b beta https://github.com/flutter/flutter.git
```

也可以使用中国的镜像，使用方法：[Using Flutter in China](https://github.com/flutter/flutter/wiki/Using-Flutter-in-China)

![GitCloneFlutter](http://huihut-img.oss-cn-shenzhen.aliyuncs.com/GitCloneFlutter.png)

### 配置环境变量

添加 `flutter\bin` 的完整路径到`用户变量` `Path`，如：

```
D:\code\git\flutter\bin
```

### 安装依赖

打开`cmd`或者`PowerShell`，安装

```
flutter doctor
```

安装过程可能持续比较久

![flutterdoctor](http://huihut-img.oss-cn-shenzhen.aliyuncs.com/flutterdoctor.png)

### 为 Android Studio 安装 Flutter 插件

![AndroidStudioInstallFlutter](http://huihut-img.oss-cn-shenzhen.aliyuncs.com/AndroidStudioInstallFlutter.png)

装好后重启 Android Studio

### 创建 Flutter APP

1. 选择 `File` > `New Flutter Project`
2. 选择 `Flutter application`
3. 输入应用名 `flutter_app`，输入 `Flutter SDK` 路径（如我的：`D:\code\git\flutter`）
4. `Finish`
5. 等待创建

创建好后，应用程序的代码在`lib/main.dart`

### 启动 Flutter APP

1. 选择设备
2. `Run`

![ASRunFlutterAPP](http://huihut-img.oss-cn-shenzhen.aliyuncs.com/ASRunFlutterAPP.png)

### 尝试热重载

把`lib/main.dart`里面的字符串

`'You have pushed the button this many times:'`

改成

`'Hahaha, You have clicked the button this many times:'`

不用停止模拟器，直接 `Save All（cmd-s/ ctrl-s）`，或者单击 `Hot Reload` 按钮（带有闪电图标的按钮）

就能看到模拟器中间那行字符串很快更新了。

嗯，体验还是蛮爽的！

## MacOS + VS Code

接下来看下在 MacOS 下的安装

### 获取 Flutter SDK

```
git clone -b beta https://github.com/flutter/flutter.git
export PATH=`pwd`/flutter/bin:$PATH
```

### 配置环境变量

打开环境变量配置文件

```
sudo vi $HOME/.bash_profile
```

添加一行你的`flutter/bin`的绝对路径，如我的：

```
export PATH=/Users/xx/code/git/flutter/bin:$PATH
```

刷新

```
source $HOME/.bash_profile
```

验证一下是否配置好

```
echo $PATH
```

### 安装依赖

```
flutter doctor
```

### 为 VS Code 安装 Flutter 插件

在扩展商店中搜索 `Dart Code` 下载安装

![](http://huihut-img.oss-cn-shenzhen.aliyuncs.com/VSCodeInstallDartCode.jpg)

安装好后重新打开

点击 `查看` - `命令面板`，搜索 `Flutter: Run Flutter Doctor` 

如果 VS Code 找不到 Flutter SDK 路径，就点击进行配置

配置好后重新 `Flutter: Run Flutter Doctor`，检查配置是否成功

### 编译 iOS 需要 Xcode 9.0.0+

接下来再检查还需要安装什么

```
flutter doctor
```

然后突然发现。。。

![MacFlutterDoctorRequiresXcode](http://huihut-img.oss-cn-shenzhen.aliyuncs.com/MacFlutterDoctorRequiresXcode.jpg)

要编译 iOS 应用需要 Xcode 9.0.0+ ！

可是官网明明说的是 Xcode 7.2+ ？打脸？？

![FlutterOfficialSiteXcode](http://huihut-img.oss-cn-shenzhen.aliyuncs.com/FlutterOfficialSiteXcode.jpg)

对此，我特意把我的 Xcode 从 7.1 更新到 7.2，结果还是说需要 9.0.0+，看来是官网教程没更新了。

![MacFlutterDoctorRequiresXcode9](http://huihut-img.oss-cn-shenzhen.aliyuncs.com/MacFlutterDoctorRequiresXcode9.0.jpg)

什么？你说为什么不更新到 Xcode 最新版？

这个......由于博主的是黑苹果，无法升级 MacOS，现在还是 OSX 10.10.5，最高支持的 Xcode 是 7.2.1

所以只好再编译 Android 应用了，下面也只讲构建 Android 的

这里附上有编译 iOS 的教程：[Flutter基础—开发环境与入门](http://blog.csdn.net/hekaiyou/article/details/52874796?locationNum=4&fps=1)


### 创建并执行 Flutter APP

在 VS Code 的终端（或其他终端）上跳转到要创建项目的路径，然后创建名为`flutter_app`的项目

```
cd ~/code/Android
flutter create flutter_app
cd flutter_app
flutter run
```

在 Windows 的时候使用的是模拟器，现在用真机体验一下

![MacFlutterRun](http://huihut-img.oss-cn-shenzhen.aliyuncs.com/MacFlutterRun.jpg)

运行后，试着修改字符串

`'You have pushed the button this many times:'`

改成

`'yeah, You have pushed the button this many times:'`

保存，然后在终端按 `r` 热重载

* `r`：热重载
* `R`：重启整个APP
* `h`：帮助
* `q`：退出APP  


嗯，Cool。