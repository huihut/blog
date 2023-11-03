---

title: Windows编译构建CEF发行版
subtitle: Windows编译构建CEF发行版
date: 2020-03-07 00:46:00
author: huihut
tags:
	- CEF
categories: 
	- CS
	
---


## 下载

在 CEF 发行版网站（[Chromium Embedded Framework (CEF) Automated Builds](http://opensource.spotify.com/cefbuilds/index.html)）下载对应的 Standard Distribution 版本，本文以 CEF 75.1.14（[cef_binary_75.1.8+g84fed5d+chromium-75.0.3770.100_windows32.tar.bz2](http://opensource.spotify.com/cefbuilds/cef_binary_75.1.8%2Bg84fed5d%2Bchromium-75.0.3770.100_windows32.tar.bz2)）为例

<!-- more -->

## 使用 CMake 构建

打开 cmake-gui，设置源码和生成路径：

* Where is the source code: `D:/code/git/cef-builds/cef_binary_75.1.14+gc81164e+chromium-75.0.3770.100_windows32`
* Where to build the binaries: `D:/code/git/cef-builds/cef_binary_75.1.14+gc81164e+chromium-75.0.3770.100_windows32/build`

点击 Configure 生成配置，修改：

* 生成动态库（个人偏好）：`CEF_RUNTIME_LIBRARY_FLAG` 从 `/MT` 改为 `/MD`
* 不用沙盒（原因见 [这里](https://bitbucket.org/chromiumembedded/cef/wiki/Tutorial#markdown-header-windows-build-steps)）：不勾选 `USE_SANDBOX`

点击 Configure、Generate 生成 VS 工程

## 使用 VS 编译

ceftests 项目会有文件格式错误
```
1>------ 已启动生成: 项目: ceftests, 配置: Debug Win32 ------
1>os_rendering_unittest.cc
1>D:\code\git\cef-builds\cef_binary_75.1.14+gc81164e+chromium-75.0.3770.100_windows32\tests\ceftests\os_rendering_unittest.cc(794): error C2220: 警告被视为错误 - 没有生成“object”文件
1>D:\code\git\cef-builds\cef_binary_75.1.14+gc81164e+chromium-75.0.3770.100_windows32\tests\ceftests\os_rendering_unittest.cc(794): warning C4819: 该文件包含不能在当前代码页(936)中表示的字符。请将该文件保存为 Unicode 格式以防止数据丢失
1>D:\code\git\cef-builds\cef_binary_75.1.14+gc81164e+chromium-75.0.3770.100_windows32\tests\ceftests\os_rendering_unittest.cc(955): error C2001: 常量中有换行符
1>D:\code\git\cef-builds\CEF 79.1.38\cef_binary_75.1.14+gc81164e+chromium-75.0.3770.100_windows32\tests\ceftests\os_rendering_unittest.cc(998): error C2001: 常量中有换行符
1>D:\code\git\cef-builds\cef_binary_75.1.14+gc81164e+chromium-75.0.3770.100_windows32\tests\ceftests\os_rendering_unittest.cc(998): fatal error C1075: “{”: 未找到匹配令牌
1>已完成生成项目“ceftests.vcxproj”的操作 - 失败。
```

用记事本打开 `os_rendering_unittest.cc` 文件，另存为 `带有 BOM 的 UTF-8` 编码，覆盖原文件。

重新编译则会编译通过。

然后设置 `cefclient` 为启动项目，F5，则可看到 Google 为主页的一个浏览器 Demo