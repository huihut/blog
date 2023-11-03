---

title: gcc/mpicc 编译器 undefined reference to ... 问题的解决
subtitle: gcc/mpicc 编译器 undefined reference to ... 问题的解决
date: 2017-07-18 09:03:48
author: huihut
tags:
	- 链接装载库
	- C/C++
categories: 
	- CS
	
---

# 描述

我的代码中使用了`libcstl`库，在代码中声明：

	#include <cstl/cmap.h>
	
可是出现这个问题，无法识别`libcstl`库里调用的函数，如下图：

<!-- more -->

![](http://huihut-img.oss-cn-shenzhen.aliyuncs.com/UndefinedReference.png)

## 猜想

1. `#include <cstl/cmap.h>`这句话不报错说明库已经装好，环境变量也没有问题。
2. 可能是链接时的错误。

## 解决

查看`Makefile`文件发现忘记添加cstl的链接库了。

	-lcstl

原来的代码是：

	mpicc -c main.c -std=c99 -lstdc++ -fopenmp -lm -o main.o
	
应该改为：

	mpicc -c main.c -std=c99 -lcstl -lstdc++ -fopenmp -lm -o main.o

![](http://huihut-img.oss-cn-shenzhen.aliyuncs.com/UndefinedReferenceMakefile.png)

如上图，改好之后就解决了！