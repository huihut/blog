---

title: CloudCompare插件编写二（数据结构）
subtitle: CloudComapre插件的编写，理解CloudComapre插件编写及点云数据结构，实现qSAF插件
date: 2017-04-27 12:35:43
author: huihut
tags:
	- QT
	- CloudCompare
categories: 
	- CS
	
---

## 唠叨

本文分三篇来介绍一个完整的CloudComapre插件的编写教程，分别是[插件框架篇](https://blog.huihut.com/2017/04/27/CloudCompareSAFPlugin_1_Framework/)、[数据结构篇](https://blog.huihut.com/2017/04/27/CloudCompareSAFPlugin_2_DataStructure/)、[算法实现篇](https://blog.huihut.com/2017/04/27/CloudCompareSAFPlugin_3_Algorithm/)。

这是第二篇，**数据结构篇**，你可以根据本文改成自己的插件，待卿临幸。

**特别注意**：本文的CloudCompare源码构建的是Qt工程并使用Qt Creator开发，并不是Visual Studio。

qSAF源码：[Github . qSAF](https://github.com/huihut/qSAF)


## 前文概要

在上回中，我们已经实现了插件的框架，现在要在`doAction()`中写插件的具体实现。

## 插件需求

我们要做的是一个qSAF(Scan Angle Filter)插件，它可以过滤给定范围内点的扫描角度。

也就是用户输入两个角度值，如`20`度、`70`度，

过滤输出每个点的扫描角度在大于等于`20`度、小于等于`70`度的范围的点云。

<!-- more -->

## 需求分析

要实现这个功能，我们需要有一个界面，可以让**用户输入两个角度**，然后获取两个角度值，接着**遍历每个点**，获取**每个点的扫描角度**，然后获得角度在**大于等于`20`度、小于等于`70`度范围**的点云，显示输出。

简单地说，就是要：

1. 输入界面
2. 遍历角度
3. 输出点云

`1. 输入界面`是QT基础；`3. 输出点云`是CC套路；只有`2. 遍历角度`有点纠结。

因此本文第二篇主要介绍`2. 遍历角度`，即介绍点云中点的数据结构。

***注意：不同类型文件的数据结构不同，本文以激光雷达文件(`.las`)来做介绍。***


## las文件的读入

首先，我们从宇宙的起源开始说起……

额，还是从`.las`文件的读入开始说起吧~

`.las`文件的读入首先进过`FileIOFilter`这个类，判断是雷达文件(`.las`)后，进入`LASFilter`类，并从它的`loadFile()`函数读入。

先看下`loadFile()`函数声明：

	virtual CC_FILE_ERROR loadFile(QString filename, ccHObject& container, LoadParameters& parameters) override;
	
特别注意三个传入参数！我就是忽视了这里才找了好久。。。

* `QString filename` 是点云文件名（包括路径）
* `ccHObject& container` 是一个实体（ccHObject），可以添加点云（ccPointCloud）
* `LoadParameters& parameters` 是选择读入文件后提示要勾选雷达的哪些信息

然后看下`loadFile()`函数体

`.las`文件首先从io流读入，再使用`liblas`这个外部库存储：

	liblas::Reader reader(liblas::ReaderFactory().CreateWithStream(ifs));
	
这里说下`liblas`：

`liblas`是用于读取和编写非常常见的LAS LiDAR格式的C/C++库，我们使用它来做对LAS的直接读取。

官网如下：

> <https://www.liblas.org/>

然后把`liblas`读入的文件进行各种处理和封装，最终封装成`ccPointCloud`

	ccPointCloud* loadedCloud = 0;
	
	int sfIndex = loadedCloud->addScalarField(field->sf);
	...
	loadedCloud->setName(chunkName);
	...
	loadedCloud->setMetaData(LAS_SCALE_X_META_DATA, QVariant(lasScale.x));
	...
	loadedCloud->addPoint(P);
	...


然后通过：

	container.addChild(loadedCloud);
	
添加到`ccHObject`中

所以：**点云的信息，都是存储在ccPointCloud中的！**

而**扫描角度存储在`ccPointCloud`的标量域中(`ccScalarField`)**

## ccPointCloud

前面已经说了很多`ccPointCloud`了，它就是CloudCompare中存储点云的类。

我们看看它的说明

	//! A 3D cloud and its associated features (color, normals, scalar fields, etc.)
	/** A point cloud can have multiple features:
		- colors (RGB)
		- normals (compressed)
		- scalar fields
		- an octree strucutre
		- per-point visibility information (to hide/display subsets of points)
		- other children objects (meshes, calibrated pictures, etc.)
	**/

我要的扫描角度就在`scalar fields`

然而在`ccPointCloud`没有直接的方法获得众多标量域中的扫描角度

终于在它的父类`ChunkedPointCloud`中发现了

## ChunkedPointCloud

`ccPointCloud`的父类`ChunkedPointCloud`中有如下两个函数：

	# 通过标量域名字获得其在标量域数组中的索引
	int ChunkedPointCloud::getScalarFieldIndexByName(const char* name) const
	
	# 通过索引获得特定标量域的指针
	ScalarField* ChunkedPointCloud::getScalarField(int index) const

通过这两个函数就可以获得指向扫描角度的指针了，要想访问扫描角度中每个点的值，需要使用`ScalarField`父类`GenericChunkedArray`的方法

## GenericChunkedArray

	# 通过每个点的索引访问特定标量域的每个的的值
	inline const ElementType* getValue(unsigned index) const
	

## LASOpenDlg

标量域中扫描角度的名字可以在`LASOpenDlg.h`中找到

	"Scan Angle Rank"
	
## 整理下思路

1. 用`Scan Angle Rank`，通过`getScalarFieldIndexByName()`获得扫描角度在标量域中的索引
2. 用索引，通过`getScalarField()`获得扫描角度标量域指针
3. 用指针，通过`getValue()`获得每个点的值

这样就获取到了每个点的扫描角度值，然后：

4. 比较扫描角度值与用户输入区间的大小，把合适的值存储起来
5. 把合适值封装成点云实体
6. 显示在界面上

上面整理的思路在下篇实现，现在我们已经知道怎么获取点云中扫描角度的值了，那其他信息呢？

## 点云其他信息的获取

看下在QT的调试信息：

![](http://huihut-img.oss-cn-shenzhen.aliyuncs.com/ccPointCloudDataStructure.jpg)

我们可以发现，其实点云的信息都能在`ccPointCloud`中获取，比如点容量、点数量、点坐标、标量域、颜色值等。

其中，标量域`vector`中有9项信息，存储的标量域顺序为：

* [0] Point Source ID
* [1] Scan Angle Rank
* [2] Flightline Edge
* [3] Scan Direction
* [4] Number of Returns
* [5] Return Number
* [6] Time
* [7] Intensity
* [8] Classification

至于如何获取每种数据，都有相应的方法实现，不是在`ccPointCloud`，就是在它的父类中，耐心点总能找到的~

## 下篇概要

下篇是算法实现篇，主要说了qSAF插件的具体实现，包括上面说的：

1. 输入界面
2. 遍历角度
3. 输出点云

请戳这里：

[CloudComapre插件编写三（算法实现）](https://blog.huihut.com/2017/04/27/CloudCompareSAFPlugin_3_Algorithm/)