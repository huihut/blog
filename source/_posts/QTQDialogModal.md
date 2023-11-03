---

title: QT QDialog 中模态与非模态对话框的研究
subtitle: QT构建错误 error expected at end of declaration list
date: 2017-06-29 20:05:27
author: huihut
tags:
	- QT
categories: 
	- CS
	
---

## 模态与非模态对话框

### 模态对话框

**模态对话框**是只能首先对其进行操作后才能操作其他窗口的对话框。实质是父线程在子线程创建这个模态对话框后，父线程阻塞，等待子线程的操作。

### 非模态对话框

**非模态对话框**则是可以同时对它和其他窗口进行操作的对话框。实质是父子进程并行运行。

<!-- more -->

## QDialog

### 概述

> The QDialog class is the base class of dialog windows.

QDialog 类是对话框窗口的基类。用于创建对话窗口。

### 继承关系

	class Q_WIDGETS_EXPORT QDialog : public QWidget
	

## QT 窗口模态

### 模态种类

	enum WindowModality {
        NonModal,
        WindowModal,
        ApplicationModal
    };
    

状态     |  Constant            | Value |  描述  | QDialog代表方法
---     |   ---                |  ---  |  ---   | ---
非模态   | Qt::NonModal         |  0    | 窗口不是模态，不会阻止其他窗口的操作 | QDialog::show()
窗口模态  | Qt::WindowModal      |  1    | 窗口对单个窗口层次结构是模态，会阻止对其所有长辈（父窗口、祖父窗口、他们的兄弟姐妹）的操作，其子窗口不会阻止 | QDialog::open()
应用模态  | Qt::ApplicationModal |  2    | 窗口对应用程序是模态，并阻止对所有窗口的操作 | QDialog::exec()


**注意**：窗口模态与应用模态都属于模态，只是`WindowModal`对局部模态，`ApplicationModal`对整个程序模态。



### 设置模态

#### 定义

	class Q_WIDGETS_EXPORT QWidget : public QObject, public QPaintDevice
	{
	public:
		void setWindowModality(Qt::WindowModality windowModality);
	}

#### 使用

	QDialog dialog;
	dialog.setWindowModality(Qt::ApplicationModal);
