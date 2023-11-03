---

title: Linux 下 CLion 编写调用 C++ 共享库
subtitle: Linux 下 CLion 编写调用 C++ 共享库
date: 2018-07-20 22:35:10
author: huihut
tags:
	- C/C++  
categories: 
	- CS
	
---

## 编写 MySharedLib 共享库

创建一个名为 MySharedLib 的共享库

CMakeLists.txt
```cmake
cmake_minimum_required(VERSION 3.10)
project(MySharedLib)

set(CMAKE_CXX_STANDARD 11)

add_library(MySharedLib SHARED library.cpp library.h)
```

<!-- more -->

library.h
```cpp
#ifndef MYSHAREDLIB_LIBRARY_H
#define MYSHAREDLIB_LIBRARY_H

// 打印 Hello World!
void hello();

// 使用可变模版参数求和
template <typename T>
T sum(T t)
{
return t;
}
template <typename T, typename ...Types>
T sum(T first, Types ... rest)
{
return first + sum<T>(rest...);
}

#endif
```

library.cpp
```cpp
#include <iostream>
#include "library.h"

void hello() {
std::cout << "Hello, World!" << std::endl;
}
```

## 被 TestSharedLib 可执行项目调用

创建一个名为 TestSharedLib 的可执行项目

CMakeLists.txt
```cmake
cmake_minimum_required(VERSION 3.10)
project(TestSharedLib)

# C++11 编译
set(CMAKE_CXX_STANDARD 11)

# 头文件路径
set(INC_DIR /home/xx/code/clion/MySharedLib)
# 库文件路径
set(LIB_DIR /home/xx/code/clion/MySharedLib/cmake-build-debug)

include_directories(${INC_DIR})
link_directories(${LIB_DIR})
link_libraries(MySharedLib)

add_executable(TestSharedLib main.cpp)

# 链接 MySharedLib 库
target_link_libraries(TestSharedLib MySharedLib)
```

main.cpp
```cpp
#include <iostream>
#include "library.h"
using std::cout;
using std::endl;

int main() {

hello();
cout << "1 + 2 = " << sum(1,2) << endl;
cout << "1 + 2 + 3 = " << sum(1,2,3) << endl;

return 0;
}
```

执行结果
```
Hello, World!
1 + 2 = 3
1 + 2 + 3 = 6
```