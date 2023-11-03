---

title: CloudCompare插件编写一（插件框架）
subtitle: CloudComapre插件的编写，理解CloudComapre插件编写及点云数据结构，实现qSAF插件
date: 2017-04-27 18:11:53
author: huihut
tags:
	- QT
	- CloudCompare
categories: 
	- CS
	
---

## 唠叨

本文唠叨了些CloudComapre这个开源软件的插件编写。

虽然这篇是入门教程，但是作为一只有梦想的程序猿，怎能像很多入门教程那样写个残缺的入门教程呢！

所以这是一个完整插件的入门教程，我们要写的插件是qSAF(Scan Angle Filter)，这是可以过滤给定范围内点的扫描角度的插件。

下面分三篇来介绍，分别是[插件框架篇](https://blog.huihut.com/2017/04/27/CloudCompareSAFPlugin_1_Framework/)、[数据结构篇](https://blog.huihut.com/2017/04/27/CloudCompareSAFPlugin_2_DataStructure/)、[算法实现篇](https://blog.huihut.com/2017/04/27/CloudCompareSAFPlugin_3_Algorithm/)。

这是第一篇，**插件框架篇**，你可以根据本文改成自己的插件，待卿临幸。

**特别注意**：本文的CloudCompare源码构建的是Qt工程并使用Qt Creator开发，并不是Visual Studio。

qSAF源码：[Github . qSAF](https://github.com/huihut/qSAF)


## CloudComapre

CloudComapre是一个开源的3D点云和网格的处理软件，开可以处理各类点云格式的数据。

> * 官网：<http://www.cloudcompare.org/>
> * Github：<https://github.com/cloudcompare/cloudcompare>


<!-- more -->


![](http://www.cloudcompare.org/images/snapshot2_small.jpg)


## CloudComapre插件编写

### 使用qDummyPlugin插件模板创建自己的插件

【2018年5月30日改：现在Github上CloudComapre仓库的master分支已经移除qDummyPlugin插件，取而代之的是 ExamplePlugin插件： [CloudCompare/plugins/example/ExamplePlugin](https://github.com/CloudCompare/CloudCompare/tree/master/plugins/example/ExamplePlugin) ，原qDummyPlugin插件可在2.9.x分支或者其他早期分支上找到：[qDummyPlugin](https://github.com/CloudCompare/CloudCompare/tree/2.9.x/plugins/qDummyPlugin)】

首先在Github上git下CloudComapre的源码，

再到 `CloudComapre/plugins/qDummyPlugin` 下

这个`qDummyPlugin`就是个插件的模板，用它来写自己的插件。
	
我们把这个模板插件文件夹拷贝一份，改为自己的插件名`qSAF`（当然，你也可以改为其他你喜欢的，以下不做累述）

现在`qSAF`里面有如下几个文件

![qSAFFile.jpg](http://huihut-img.oss-cn-shenzhen.aliyuncs.com/qSAFFile.jpg)

我们需要做的是**把里面的`qDummyPlugin`及类似的名字改为自己的`qSAF`**。

**注意：文件名和文件里面内容都要修改！**

如：

原本`CMakeLists.txt`文件里面是这样的：

	cmake_minimum_required(VERSION 3.0)

	#REPLACE ALL 'DUMMY' OCCURENCES BY YOUR PLUGIN NAME
	#AND ADAPT THE CODE BELOW TO YOUR OWN NEEDS!

	option( INSTALL_QDUMMY_PLUGIN "Check to install qDUMMY plugin" OFF )

	# CloudCompare 'DUMMY' plugin
	if (INSTALL_QDUMMY_PLUGIN)
    	project( QDUMMY_PLUGIN )
    
    	#load necessary libraries (see qPCV for an example)
    	#add_subdirectory (LIB1)
    
    	#if the plugin is an 'OpenGL filter', uncomment the line below
    	#set( CC_OPENGL_FILTER ON BOOL)
    	include( ../CMakePluginTpl.cmake )
    
    	#set dependencies to necessary libraries (see qPCV for an example)
    	#target_link_libraries( ${PROJECT_NAME} LIB1 )
    	#include_directories( ${LIB1_INCLUDE_DIR} )
	endif()

修改后的`CMakeLists.txt`文件里面是这样的：

	cmake_minimum_required(VERSION 3.0)

	option( INSTALL_QSAF_PLUGIN "Check to install qSAF plugin" OFF )

	if (INSTALL_QSAF_PLUGIN)

    	#CloudCompare ‘SAF’ plugin

    	project( QSAF_PLUGIN )
    
    	include( ../CMakePluginTpl.cmake )
    
	endif()

剩下的`qSAF.h`、`qSAF.cpp`和`qSAF.qrc`就不一一列出了

修改后变成这样：

![qSAFFile2](http://huihut-img.oss-cn-shenzhen.aliyuncs.com/qSAFFile2.jpg)

### 使用qmake构建pro文件

在终端进入到你的插件的`qSAF`目录，执行：

	qmake -project -r CMakeLists.txt
	
就会在`qSAF`目录下构建一个项目的pro文件。


### 导入工程到QT

现在把CloudComapre导入到QT，

在`CloudComapre-plugins-plugins.pro`中，加上自己的插件：

![](http://huihut-img.oss-cn-shenzhen.aliyuncs.com/QTqSAFPluginsPro.jpg)

保存刷新后就会在项目上看到了。

### pro文件编写

由于qmake自动生成的pro文件不适合我们要写的插件，所以要自己修改pro文件。

pro文件的编写可以参考：

[QT的pro文件编写](https://blog.huihut.com/2017/04/26/QTProFile/)

里面的常规pro文件就是qSAF的。

里面的路径可以不用修改，具体看你项目的Release生成的位置，

***特别注意：指定生成路径中的libs和plugins要正确***

### 完成模板插件框架

没错！这就完成了，你的插件已经做出来了！现在可以Run一下或者Debug一下看看啦~

选中点云，使用`qSAF`，会这样：

![](http://huihut-img.oss-cn-shenzhen.aliyuncs.com/qSAFShoudNotBeUsedAsIs.jpg)

莫慌！这是正常现象，因为你的`qSAF`具体实现还没有写呢！


### 遇到问题

#### 1. 编译出错

可能原因：

* `qSAF.h`、`qSAF.cpp`和`qSAF.qrc`这三个文件里面的**`qDummyPlugin`及类似的名字要改为自己的`qSAF`**，如果改错或没改，可能报错。
* 自己写的代码有错，这个视情况而定

#### 2. 运行的CloudComapre插件上没有你编写的插件的快捷方式

可能原因

* 插件的生成路径出错了，自己插件的pro文件中的plugins指定生成路径要正确。

### 个性化插件

现在来个性化一下我们的插件

在`qSAF.h`中：

	// 设置插件的唯一ID
	Q_PLUGIN_METADATA(IID "cccorp.cloudcompare.plugin.qSAF")

	// 设置插件名字
	virtual QString getName() const override { return "SAF"; }
	
	// 设置插件的描述
	virtual QString getDescription() const override { return "Filter the scanning angle in a range of points"; }
	
	// 设置插件图标，这个要在 qSAF.cpp 里设置图标路径
	virtual QIcon getIcon() const override;
	
	// 设置插件要执行的操作（重点）
	void doAction();


### 插件doAction()

我们编写插件是要做些事情，在`CloudComapre`插件中就是在doAction()中实现按下插件`SAF`后要做的事。

这个函数在`qSAF.cpp`中，你会发现复制的模板插件的`doAction()`是这样的（把`qDummyPlugin`改为`qSAF`啦）：

	void qSAF::doAction()
	{
		//m_app should have already been initialized by CC when plugin is loaded!
		//(--> pure internal check)
		assert(m_app);
		if (!m_app)
			return;

		/*** HERE STARTS THE ACTION ***/

		//put your code here
		//--> you may want to start by asking parameters (with a custom dialog, etc.)

		//This is how you can output messages
		m_app->dispToConsole("[qSAF] Hello world!",ccMainAppInterface::STD_CONSOLE_MESSAGE); //a standard message is displayed in the console
		m_app->dispToConsole("[qSAF] Warning: qSAF plugin shouldn't be used as is!",ccMainAppInterface::WRN_CONSOLE_MESSAGE); //a warning message is displayed in the console
		m_app->dispToConsole("qSAF plugin shouldn't be used as is!",ccMainAppInterface::ERR_CONSOLE_MESSAGE); //an error message is displayed in the console AND an error box will pop-up!

		/*** HERE ENDS THE ACTION ***/

	}
	
	
我们要做的就是在

	/*** HERE STARTS THE ACTION ***/
	
下面写自己的插件代码。

刚才你看的错误信息就是这句：

	m_app->dispToConsole("qSAF plugin shouldn't be used as is!",ccMainAppInterface::ERR_CONSOLE_MESSAGE); 
	
这是控制台输出的错误信息。

错误信息(`ERR_CONSOLE_MESSAGE`)同时在控制台和窗体形式出现，而其他标准信息(`STD_CONSOLE_MESSAGE`)、警告信息(`WRN_CONSOLE_MESSAGE`)，则只在控制台显示。

现在删掉`/*** HERE STARTS THE ACTION ***/`下面的，改为自己的一句：

	m_app->dispToConsole("[qSAF] 程序是从错误开始的！",ccMainAppInterface::ERR_CONSOLE_MESSAGE);
	
结果如下：

![](http://huihut-img.oss-cn-shenzhen.aliyuncs.com/qSAFStartFromError.jpg)

## 插件框架完成

现在已经完成了插件框架的编写啦。

其实只要使用`CloudComapre`提供的插件模板`qDummyPlugin`，改成自己的就可以啦。

现在可以发挥你的想象力，在`doAction()`里面随心所欲地做各种事情啦。

但是只有框架还不够，要想对点云进行操作，和SAF功能的具体实现，还需要了解`CloudComapre`中点云的数据结构：

[CloudComapre插件编写二（数据结构）](https://blog.huihut.com/2017/04/27/CloudCompareSAFPlugin_2_DataStructure/)

如果你对点云数据结构虐它如虐狗，可以戳这里：

[CloudComapre插件编写三（算法实现）](https://blog.huihut.com/2017/04/27/CloudCompareSAFPlugin_3_Algorithm/)