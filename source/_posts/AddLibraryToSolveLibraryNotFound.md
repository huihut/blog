---

title: 通过直接添加或者手动编译添加库，解决 library not found for -lxxx 等的问题
subtitle: 通过手动编译添加库，解决 library not found for -lxxx 的问题
date: 2017-08-23 10:48:26
author: huihut
tags:
	- 链接装载库
categories: 
	- CS
	
---

## 前言

本文基本上能完美解决这种库文件无法找到问题。

文中以`IceUtil`库为例子，你可以推广到其他库，方法相同。

## 问题描述

在 Mac 下用 Homebrew 安装 ZeroC Ice 这个中间件后发现 IceUtil 库缺失，IDE 报了个链接错误，如下图：

<!-- more -->

![](http://huihut-img.oss-cn-shenzhen.aliyuncs.com/LibraryNoFoundForIIceUtil.jpg)

## 问题分析

这个问题是说链接器在链接的时候找不到 IceUtil 这个库，那我们就告诉它（添加库的路径），让它找到就好啦！

扩展：

* 静态库无法链接报错：

		library not found for -lxxx
	
* 动态库无法装载报错：

		dyld: library not loaded …/libxxx.dylib

## 解决办法

### 方法一：直接添加库

通常解决办法是：库存在，直接添加路径。

也就是通常遇到这个问题的时候，库是已经编译安装好了的，但是 IED 不能找到。这样的话就直接添加库的路径就好了。

#### 第一步：找库

第一步，自己找到这个库。

库一般放在系统默认处或者安装到特定地方。

1. Linux 系统默认库放在：

		/lib
		/usr/lib
		/usr/local/lib
		...
	
2. 安装到特定地方，如我 Mac 的用 Homebrew 安装到：

		/usr/local/Cellar/ice/3.7.0/lib

#### 第二步：添加路径 

添加库一般以下三种方法任选其一：


1. 系统环境变量添加
	1. 系统级：修改`/etc/profile`或者`/etc/bashrc`
	2. 用户级：修改`~/.bashrc`或者`~/.bash_profile`

```
#添加库的bin文件夹路径
export PATH =$PATH:$HOME/bin

#添加到gcc头文件
export C_INCLUDE_PATH=$C_INCLUDE_PATH:/MyLib

#添加到g++头文件路径
export CPLUS_INCLUDE_PATH=$CPLUS_INCLUDE_PATH:/MyLib

#添加到动态库
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/MyLib

#添加到静态库
export LIBRARY_PATH=$LIBRARY_PATH:/MyLib
```	


2. IED 编译环境添加

	因 IDE 不同而不同，如 Qt 在`项目`-`构建设置`-`构建环境`处添加
	
	具体请参考：[QT 无法链接动态库 dyld library not loaded ... libhdf5.100.dylib](https://blog.huihut.com/2017/06/05/QtDyldLibraryNotLoaded/)

3. 代码添加

		# 如 Qt pro 文件添加
		LIBS += -L/usr/local/Cellar/ice/3.7.0/lib -lIceUtil

这样，IDE就能找到库啦！

### 方法二：手动编译添加库

若是你的库不存在，也就是说安装的时候没有编译生成这个库文件或者安装后莫名的不见了，这样只能重新安装或者手动编译添加库。以下讲手动编译添加库。

思路是：找库，如果找不到，手动编译生成库文件，拷贝到库文件目录，用上面`添加路径`的方法添加路径让IDE找到。

#### 第一步：找库

对的，还是要找找的，不然怎么知道没有呢！[捂脸]

可以在一些常放库的文件夹下找，尽量靠近根目录，如：
	
	sudo find /usr -name "libIceUtil*"
	
扩展：

如果找到名为`libIceUtil.3.7.0.a`的库，但是找不到`libIceUtil.a`的库，可以试着拷贝`libIceUtil.3.7.0.a`库成名为`libIceUtil.a`的库

	cp ./libIceUtil.3.7.0.a ./libIceUtil.a

这个方法适用于：

* 同版本下，缺失没版本号的库文件
* 不同版本下，库文件没因为版本的改变而发生改变

#### 第二步，手动编译

	#因为IceUtil是Ice的库，所以克隆下Ice来
	git clone https://github.com/zeroc-ice/ice.git
	
	#因为我需要的是C++版
	cd ice/cpp
	
	#直接编译
	make
	
编译好后就能找到这个库了：`cpp/lib/libIceUtil.a`

![](http://huihut-img.oss-cn-shenzhen.aliyuncs.com/MakeLibIceUtil.jpg)

其他库请参考官方的编译安装教程

附：[Building Ice for C++ on macOS](https://github.com/zeroc-ice/ice/blob/master/cpp/BuildInstructionsMacOS.md)

#### 第三步，拷贝库到安装后的文件夹

到`ice/cpp/lib`目录下：

	 cp ./libIceUtil.a /usr/local/Cellar/ice/3.7.0/lib
	 
#### 第四步：添加路径

上面已经说了三种方法，这里直接代码添加：

 	# 如 Qt pro 文件添加
	LIBS += -L/usr/local/Cellar/ice/3.7.0/lib -lIceUtil
	
这样就解决了！

## 唠叨

以上这些方法基本上能完美解决这种库文件无法找到问题，如果有本文没有提及的欢迎留言讨论。