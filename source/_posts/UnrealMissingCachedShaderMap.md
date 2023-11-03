---

title: Unreal 源码编译运行 Missing cached shader map... 的问题
subtitle: Unreal 源码编译运行 Missing cached shader map... 的问题
date: 2017-07-28 14:10:28
author: huihut
tags:
	- Unreal
categories: 
	- CS
	
---

## Unreal 源码编译运行 Missing cached shader map... 的问题

### 问题描述

Unreal 源码是 Github-release 分支，版本是4.16。

编译运行Unreal引擎源码的时候，出现虚幻编辑器的窗口，但是卡在45%不动。

<!-- more -->

![](http://huihut-img.oss-cn-shenzhen.aliyuncs.com/UnrealLunch45Missing.png)

并在调试窗口输出：

    Missing cached shader map for material WorldGridMaterial, compiling. Is special engine material.

![](http://huihut-img.oss-cn-shenzhen.aliyuncs.com/MissingCachedShaderMap.png)

### 同类问题

同样这个问题，在unrealengine社区有人遇到，但没有说明具体原因及解决方案。

[answers.unrealengine.com - UE4.7 source build, missing cached shader](https://answers.unrealengine.com/questions/184696/ue47-source-build-missing-cached-shader.html)

### 结果

结果我没有进行任何操作，**再等会儿** 居然成功运行了起来！

我猜测可能是WorldGridMaterial这个材质需要联网下载，然后由于网络问题在45%处卡住了一会儿。
