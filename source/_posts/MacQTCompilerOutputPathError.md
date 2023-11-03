---

title: Mac下QT编译输出路径错误：ld unknown option ...
subtitle: Mac下QT编译输出路径错误
date: 2017-03-26 02:30:47
author: huihut
tags:
	- 链接装载库
	- QT
categories: 
	- CS
	
---


## ERROR: ld unknown option rpath
    
Mac指定qmake的生成路径时，用 `-Wl,-rpath,$$PWD/..` 而不是 `-Wl,-rpath=$$PWD/..` 如下：

    macx{
    # linux only

    # 编译时候指定libs查找位置
    QMAKE_LFLAGS_RELEASE += -Wl,-rpath,$$PWD/../../Release/libs -Wl
    QMAKE_LFLAGS_DEBUG += -Wl,-rpath,$$PWD/../../Release/libs -Wl

    # 指定生成路径
    DESTDIR = $$PWD/../../Release
    }

Linux则用 `-Wl,-rpath=$$PWD/..`

    unix:!macx{
    # linux only

    # 编译时候指定libs查找位置
    QMAKE_LFLAGS_RELEASE += -Wl,-rpath=$$PWD/../../Release/libs -Wl,-Bsymbolic
    QMAKE_LFLAGS_DEBUG += -Wl,-rpath=$$PWD/../../Release/libs -Wl,-Bsymbolic

    # 指定生成路径
    DESTDIR = $$PWD/../../Release/libs

    }
    
<!-- more -->

## ERROR: ld unknown option Bsymbolic

Mac 不支持`-Bsymbolic`，所以不能这样：

    QMAKE_LFLAGS_RELEASE += -Wl,-rpath,$$PWD/../../Release/libs -Wl,-Bsymbolic
    
应该删去`-Bsymbolic`，如下：

    QMAKE_LFLAGS_RELEASE += -Wl,-rpath,$$PWD/../../Release/libs -Wl