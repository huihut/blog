---

title: QT 无法链接动态库 dyld library not loaded ... libhdf5.100.dylib
subtitle: 
date: 2017-06-05 16:14:05
author: huihut
tags:
	- QT
	- 链接装载库
categories: 
	- CS
	
---

## qt dyld library not loaded .../libhdf5.100.dylib


![DyldLibraryNotLoaded](http://huihut-img.oss-cn-shenzhen.aliyuncs.com/DyldLibraryNotLoaded.png)

## 分析

无法链接动态库，考虑手动添加链接。

<!-- more -->

## 找到 libhdf5.100.dylib 文件及路径

![](http://huihut-img.oss-cn-shenzhen.aliyuncs.com/hdf5-lib-dylib.jpg)

	/usr/local/opt/hdf5/lib
	
## 添加到 QT Run Environment


把路径添加到`Run Environment`中的`DYLD_LIBRARY_PATH`变量的值中。

若无此变量则添加，若已有则按编辑并在后面加上路径。

Run!

![](http://huihut-img.oss-cn-shenzhen.aliyuncs.com/QTRunEnvironment.jpg)


## 补充：添加到构建设置-构建环境中

如果添加到 `Run Environment` 中还是不行，可以添加到`Build`-`构建设置`-`构建环境`中。

Debug!

![](http://huihut-img.oss-cn-shenzhen.aliyuncs.com/QtDebugEnvironment.jpg)

## Thanks

[stackoverflow . qt mac osx 10.8 dyld: library not loaded…image not found](http://stackoverflow.com/questions/13611740/qt-mac-osx-10-8-dyld-library-not-loaded-image-not-found)