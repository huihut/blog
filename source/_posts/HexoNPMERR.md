---

title: hexo 构建静态文件无法生成 index.html 等文件
subtitle: hexo 构建静态文件无法生成 index.html 等文件的问题及其原因和解决办法
date: 2017-05-13 20:28:58
author: huihut
tags:
	- Hexo
categories: 
	- CS
	
---

## hexo g 无法生成 index

构建情况如下图：

![](http://huihut-img.oss-cn-shenzhen.aliyuncs.com/ganningZHexoNoIndex.png)

但是一些文件没有生成，如 `index.html` 文件。

<!-- more -->

## 查看 npm 安装各 hexo 插件的情况

	npm ls --depth 0


## hexo 的一些插件未安装插件

**npm ERR! missing npm ERR! missing hexo-generator-archive...**

![](http://huihut-img.oss-cn-shenzhen.aliyuncs.com/ganningZHexoNPMls.png)

## 解决

逐一安装缺失的包

	npm install hexo-generator-archive --save
	...
	
安装完后重新构建即可解决。