---

title: Makefile 问题及解决
subtitle: Makefile遇到的问题及解决方案
date: 2017-03-13 05:59:10
author: huihut
tags:
	- Makefile
categories: 
	- CS
	
---


## 问题一

makefile文件的clean出错


<!-- more -->


![](http://huihut-img.oss-cn-shenzhen.aliyuncs.com/MakefileError_Clean1.png)

### 解决

`clean`下面的那句命令没有缩进，应该用`[Tab]`缩进

![](http://huihut-img.oss-cn-shenzhen.aliyuncs.com/MakefileError_Clean3.png)

![](http://huihut-img.oss-cn-shenzhen.aliyuncs.com/MakefileError_Clean4.png)


### 小结

* 利用 [Google](https://www.google.com/)、[stackoverflow](http://stackoverflow.com/) 等含金量高的问题解决平台
* 注意语法规范

---

## 问题二

![](http://huihut-img.oss-cn-shenzhen.aliyuncs.com/MakefileError_Clean2.png)

### 解决

`newhello:hello.o hello_fn.o` 中的`newhello`应该写成`hello`，应该与`hello.c`中的名字一样

![](http://huihut-img.oss-cn-shenzhen.aliyuncs.com/MakefileError_Clean3.png)

![](http://huihut-img.oss-cn-shenzhen.aliyuncs.com/MakefileError_helloc2.png)

### 小结

* 注意编译运行的文件名