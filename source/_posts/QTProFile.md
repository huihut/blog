---

title: QT的pro文件编写
subtitle: Qt的.pro文件编写
date: 2017-04-26 18:48:31
author: huihut
tags:
	- QT
categories: 
	- CS
	
---


## 常规pro文件

下面是CloudCompare中一个自己写的插件的pro文件，

pro文件编写可按照如下方式写。


<!-- more -->


	# 添加QT的模块
	QT  +=  widgets \
        opengl

	# 指明让qmake生成哪种makefile文件，app表示应用程序，lib表示库
	TEMPLATE = lib
	
	# 指明生成的库的名字
	TARGET = qSAF
	
	# 指明编译依赖路径
	DEPENDPATH += .\
	
	# 包含头文件路径
	INCLUDEPATH += .\
                $$PWD/../

	# 工程的头文件路径
	HEADERS += qSAF.h \
           	../ccStdPluginInterface.h \
           	../ccPluginInterface.h \
           	../ccMainAppInterface.h \
    		ccSAFDlg.h
	
	# 工程的源文件路径
	SOURCES += qSAF.cpp ../ccStdPluginInterface.cpp \
    		ccSAFDlg.cpp
	
	# 工程的资源文件路径
	RESOURCES += qSAF.qrc

	#CC (CloudCompare核心算法库路径)
	win32:CONFIG(release, debug|release): LIBS += -L$$PWD/../../../Release/libs/ -lCC_CORE_LIB
	else:win32:CONFIG(debug, debug|release): LIBS += -L$$PWD/../../../Release/libs/ -lCC_CORE_LIB
	else:unix: LIBS += -L$$PWD/../../../Release/libs/ -lCC_CORE_LIB

	# 包含CC的头文件路径
	INCLUDEPATH += $$PWD/../../CC/include
	# 包含CC的依赖路径
	DEPENDPATH += $$PWD/../../CC

	#qCC_db (CloudCompare数据库路径)
	win32:CONFIG(release, debug|release): LIBS += -L$$PWD/../../../Release/libs/ -lQCC_DB_LIB
	else:win32:CONFIG(debug, debug|release): LIBS += -L$$PWD/../../../Release/libs/ -lQCC_DB_LIB
	else:unix: LIBS += -L$$PWD/../../../Release/libs/ -lQCC_DB_LIB

	INCLUDEPATH += $$PWD/../../libs/qCC_db
	DEPENDPATH += $$PWD/../../libs/qCC_db

	# 工程的ui文件路径
	FORMS += \
    		SAFDlg.ui

	# Mac系统下，则执行括号内的代码
	macx
	{
	# 编译时候指定libs查找位置
	QMAKE_LFLAGS_RELEASE += -Wl,-rpath,$$PWD/../../../Release/libs -Wl
	QMAKE_LFLAGS_DEBUG += -Wl,-rpath,$$PWD/../../../Release/libs -Wl

	#指定生成路径
	DESTDIR = $$PWD/../../../Release/CloudCompare.app/Contents/plugins
	}

	# Mac外的其他Unix系统下(Linux)，则执行括号内的代码
	unix:!macx{
	# linux only

	# 编译时候指定libs查找位置
	QMAKE_LFLAGS_RELEASE += -Wl,-rpath=$$PWD/../../../Release/libs -Wl,-Bsymbolic
	QMAKE_LFLAGS_DEBUG += -Wl,-rpath=$$PWD/../../../Release/libs -Wl,-Bsymbolic

	#指定生成路径
	DESTDIR = $$PWD/../../../Release/plugins
	}

	# Windows系统下，则执行括号内的代码
	win32 {
	# windows only

	}
