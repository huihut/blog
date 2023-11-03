---

title: Qt GDB 无法调试 MSVC 编译的程序而报错：file format not recognized
subtitle: 
date: 2018-06-02 13:11:56
author: huihut
tags:
	- QT
categories: 
	- CS
	
---

## 环境

* Windows 10 x64
* Visual Studio 2017
* Qt 5.11

## 异常

Qt Debug 时提示异常：

```
qt not in executable format. file format not recognized
```

<!-- more -->

![qt-not-in-executable-format-file-format-not-recognized](http://huihut-img.oss-cn-shenzhen.aliyuncs.com/qt-not-in-executable-format-file-format-not-recognized.PNG)

## 原因

编译器（Compiler）使用了 64 位版本的 MSVC，调试器（Debugger）却使用了 32 位的 MinGW 的 GDB，从而 GDB 不能调试 64 位程序而报错。

## 解决

* 在 Qt 的 `工具` - `选项` - `构建和运行` - `Debuggers` 选择 CDB（Debugging Tools for Windows），
* 不能自动检测到则手动添加，如：`C:\Program Files (x86)\Windows Kits\10\Debuggers\x64\cdb.exe`，
* 没有 cdb 则下载：[Windows Driver Kit (WDK)](https://docs.microsoft.com/en-us/windows-hardware/drivers/download-the-wdk)（下载装好后重启 Qt 一般就可以自动检测到）