---

title: 解决源码编译 ZeroC Ice 缺失 mcpp、bzip2、expat 库的问题
subtitle: 解决源码编译 ZeroC Ice 缺失 mcpp、bzip2、expat 库的问题
date: 2017-09-06 23:50:18
author: huihut
tags:
	- 链接装载库
	- ZeroC-Ice
categories: 
	- CS
	
---


## 缺失 mcpp 库

### 报错

    /usr/bin/ld: cannot find -lmcpp

### 解决

1. 下载最新的 nux-dextop-release*rpm 包

    <http://li.nux.ro/download/nux/dextop/el7/x86_64/>

2. 安装 nux-dextop-release 包

        sudo rpm -Uvh nux-dextop-release*rpm

3. 安装 mcpp

        sudo yum install libmcpp-devel

<!-- more -->

## 缺失 bzip2 库

### 报错

    src/Ice/ConnectionI.cpp:32:21: fatal error: bzlib.h: No such file or directory
    #  include <bzlib.h>
                 ^
    compilation terminated.
    make: *** [src/Ice/build/x64/shared/pic/ConnectionI.o] Error 1

### 解决

    sudo yum install bzip2-devel

## 缺失 expat 库

### 报错

    src/IceXML/Parser.cpp:12:19: fatal error: expat.h: No such file or directory
    #include <expat.h>
                   ^
    compilation terminated.
    make: *** [src/IceXML/build/x64/shared/pic/Parser.o] Error 1

### 解决

    sudo yum install expat-devel
