---

title: "std::map error: implicit instantiation of undefined template 不能识别std::map迭代器指针"
subtitle: "std::map error: implicit instantiation of undefined template 不能识别std::map迭代器指针"
date: 2017-07-17 16:11:06
author: huihut
tags:
	- C/C++
categories: 
	- CS
	
---

## std::map error: implicit instantiation of undefined template 不能识别std::map迭代器指针

代码如下：

    std::map<std::string, int>::iterator map_iter;
    map_iter->first

QT中报错：

    /Applications/Xcode-beta.app/Contents/Developer/Toolchains/XcodeDefault.xctoolchain/usr/include/c++/v1/utility:258: error: implicit instantiation of undefined template 'std::__1::basic_string<char, std::__1::char_traits<char>, std::__1::allocator<char> >'
    _T1 first;
        ^
        
发现是引入头文件错误。

<!-- more -->

我原本头文件引入如下：

    #include <stdio.h>
    #include <string.h>
    #include <map>
    
应该 `#include <string>`，而不是 `#include <string.h>`，修改为如下：

    #include <iostream>
    #include <string>
    #include <map>

    