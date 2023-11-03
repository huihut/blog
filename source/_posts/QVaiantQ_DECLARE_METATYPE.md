---

title: Qt的QVaiant中使用非QMetaType类型缺少Q_DECLARE_METATYPE宏定义错误
subtitle: OpenCV使用CMake和MinGW的编译安装及其在Qt配置运行
date: 2017-12-05 18:08:47
author: huihut
tags:
	- QT
categories: 
	- CS
	
---

## 报错

```
E:\Qt\Qt5.9.3\5.9.3\mingw53_32\include\QtCore\qglobal.h:738: error: static assertion failed: Type is not registered, please use the Q_DECLARE_METATYPE macro to make it known to Qt's meta-object system
 #define Q_STATIC_ASSERT_X(Condition, Message) static_assert(bool(Condition), Message)
                                               ^
```

<!-- more -->

![](http://huihut-img.oss-cn-shenzhen.aliyuncs.com/Q_DECLARE_METATYPE.png)

## 报错代码
```
const QCameraInfo &cameraInfo
...
QVariant::fromValue(cameraInfo)
```

## 原因

QVaiant 不能识别自定义类型和其他非 QMetaType 内置类型， 而 QCameraInfo 是非 QMetaType 内置类型，因此使用 `QVariant::fromValue(cameraInfo)` 之前，需要使用 `Q_DECLARE_METATYPE` 宏对 QCameraInfo 进行声明。

## 解决

在代码中加个宏定义：

```
Q_DECLARE_METATYPE(QCameraInfo)
```

## 总结

* Q_DECLARE_METATYPE
    * 如果要使自定义类型或其他非 QMetaType 内置类型在 QVaiant 中使用，必须使用该宏
    * 该类型必须有公有的 构造、析构、复制构造 函数
* qRegisterMetaType 必须使用该函数的两种情况
    * 如果非 QMetaType 内置类型要在 Qt 的属性系统中使用
    * 如果非 QMetaType 内置类型要在 queued 信号与槽 中使用

## 参考

* [Q_DECLARE_METATYPE与qRegisterMetaType学习](http://blog.sina.com.cn/s/blog_640531380100yfei.html)
* [How to use QVariant::fromValue with QString?](https://stackoverflow.com/questions/34278017/how-to-use-qvariantfromvalue-with-qstring)
* [QMetaType Class - Q_DECLARE_METATYPE(Type)](http://doc.qt.io/qt-5/qmetatype.html#Q_DECLARE_METATYPE)
