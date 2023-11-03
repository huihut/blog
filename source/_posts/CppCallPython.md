---

title: C++ 调用 Python 模块
subtitle: C++ 调用 Python 模块
date: 2018-06-12 21:28:57
author: huihut
tags:
	- Python 
	- C/C++  
categories: 
	- CS
	
---

```cpp
// C++ call Python module 
// author: huihut
// repo: https://gist.github.com/huihut/b4597d097123a8c8388c71b3f0ff21e5

#include <iostream>
#include <Python.h>

// C++ call Python module
bool CppCallPython()
{
    // Python initialize
    Py_Initialize();
    if (!Py_IsInitialized())
    {
        std::cout << "Python initialization failed!\n";
        return false;
    }

    // If my MyPython.py file is in "/Users/xx/code", set the working path to "/Users/xx/code"
    std::string path = "/Users/xx/code";
    PySys_SetPath(&path[0u]);

    // Import MyPython.py module
    PyObject* pModule = PyImport_ImportModule("MyPython");
    if (!pModule)
    {
        std::cout <<"Cannot open Python file!\n";
        return false;
    }

    // Get the HelloPython() function in the module
    PyObject* pFunhello = PyObject_GetAttrString(pModule, "HelloPython");
    if (!pFunhello)
    {
        std::cout << "Failed to get this function!";
        return false;
    }

    // Call HelloPython()
    PyObject_CallFunction(pFunhello, NULL);

    // Finalize
    Py_Finalize();

    return true;
}
```
