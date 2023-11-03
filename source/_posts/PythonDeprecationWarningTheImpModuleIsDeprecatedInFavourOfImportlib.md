---

title: Python DeprecationWarning the imp module is deprecated in favour of importlib
subtitle: Python DeprecationWarning the imp module is deprecated in favour of importlib
date: 2019-01-21 2:21:18
author: huihut
tags:
	- Python
categories: 
	- CS
	
---


## 报错

```
E:\PyCharm 2018.2.5\helpers\pycharm\docrunner.py:1: DeprecationWarning: the imp module is deprecated in favour of importlib; see the module's documentation for alternative uses
  import imp
```

## 原因

imp 从 Python 3.4 之后弃用了，建议使用 importlib 代替

<!-- more -->

## 解决

打开 `E:\PyCharm 2018.2.5\helpers\pycharm\docrunner.py` 文件，做如下两步修改：

1. 在第一行，注释掉 `imp`，导入 `importlib`

    ```python
    #import imp
    import importlib
    ```

2. 在第 230 行的 `loadSource` 函数中，注释 `imp.load_source`，使用 `importlib.machinery.SourceFileLoader` 加载模块

    ```python
    #module = imp.load_source(moduleName, fileName)
    module = importlib.machinery.SourceFileLoader(moduleName, fileName).load_module()
    ```