---

title: PyCharm自动安装pymysql包失败
subtitle: PyCharm自动安装pymysql包失败。问题描述：在PyCharm中新建Web2Py项目后提示没有pymysql，自动安装失败
date: 2017-01-21 21:41:07
author: huihut
tags:
	- Python
categories: 
	- CS
	
---

## 问题描述：    
在PyCharm中新建Web2Py项目后提示没有pymysql，自动安装失败，如图：  

<!-- more -->


![](http://huihut-img.oss-cn-shenzhen.aliyuncs.com/pymysqlError.jpg)  

## 解决方案：  

### 1. 安装pip  

在终端输入

	pip -V 

如果显示版本路径，就说明pip已经安装了  
如果没有安装pip则需要在终端安装  
  
  ①使用脚本安装pip：

* 各平台（管理员运行）：  

		python get-pip.py
    
②使用包管理器安装pip：

* Mac：  
     
    	sudo easy_install pip
    
    
* Debian & Ubuntu:  
    
    	sudo apt-get install python-pip
    
    
* Fedora:    
    
    	sudo yum install python-pip


### 2. 安装pymysql  


	pip install pymysql
  
  

### 3. 正常情况下以上两步就行了。  

然而我的项目中依然提示没有pymysql，结果发现只是Python版本选错了  
(ノ▼Д▼)ノ  
我pymysql是安装到Python2.7，而PyCharm项目是用Python2.6  
所以就到```Preferences``` > ```Project Interpreter```中调成Python2.7就行了，
  
  
### Thanks
> http://stackoverflow.com/questions/36956124/permision-issues-while-using-and-installing-python-packages