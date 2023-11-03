---

title: WinRT(C++/CX) Platform::String^ 与 std::string 的类型转换
subtitle: WinRT(C++/CX) Platform::String^ 与 std::string 的类型转换
date: 2018-08-23 23:43:00
author: huihut
tags:
	- C/C++
categories: 
	- CS
	
---

Gist 仓库地址：https://gist.github.com/huihut/aa90bd3a202090e25b9a4792c80e6920

```cpp
#include <string>

std::string Managed_Str_To_Std_Str(Platform::String^ ms)
{
    std::wstring w_str(ms->Begin());
    return std::string(w_str.begin(), w_str.end());
}

Platform::String^ Std_Str_To_Managed_Str(const std::string & input)
{
    std::wstring w_str = std::wstring(input.begin(), input.end());
    const wchar_t* w_chars = w_str.c_str();
    return (ref new Platform::String(w_chars));
}
```