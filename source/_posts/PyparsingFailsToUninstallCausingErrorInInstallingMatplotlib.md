---

title: pyparsing 无法卸载导致安装 matplotlib 出错
subtitle: pyparsing 无法卸载导致安装 matplotlib 出错
date: 2018-10-13 11:02:00
author: huihut
tags:
	- Python
categories: 
	- CS
	
---

## 表现

```
sudo pip install matplotlib
```

安装 matplotlib 时出现以下错误

```
 Found existing installation: pyparsing 1.5.6
Cannot uninstall 'pyparsing'. It is a distutils installed project and thus we cannot accurately determine which files belong to it which would lead to only a partial uninstall.
```

<!-- more -->

## 原因

安装 matplotlib 需要卸载我已安装的 1.5.6 版本的 pyparsing，再重新装新版本，但是无法卸载

## 解决

手动重装最新版 pyparsing

首先，去官网查看最新版是什么版本：<https://pypi.org/project/pyparsing/>

当前最新版是 `pyparsing 2.2.0`，所以执行如下重新安装最新版：

```
sudo pip install -I pyparsing==2.2.0
```