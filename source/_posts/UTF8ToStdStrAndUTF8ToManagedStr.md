---

title: WinRT(C++/CX) UTF8类型转换为std::string和Platform::String^的Unicode字符串
subtitle: WinRT(C++/CX) UTF8类型转换为std::string和Platform::String^的Unicode字符串
date: 2018-08-23 23:44:50
author: huihut
tags:
	- C/C++
categories: 
	- CS
	
---

Gist 仓库地址：https://gist.github.com/huihut/8f75e2332e05673ff7e1248ad5e85339

```cpp
#include <string>
#include <Windows.h>

std::string UTF8_To_Std_Str(const std::string & str)
{
	int nwLen = MultiByteToWideChar(CP_UTF8, 0, str.c_str(), -1, NULL, 0);

	wchar_t* pwBuf = new wchar_t[nwLen + 1];
	memset(pwBuf, 0, nwLen * 2 + 2);

	MultiByteToWideChar(CP_UTF8, 0, str.c_str(), str.length(), pwBuf, nwLen);

	int nLen = WideCharToMultiByte(CP_ACP, 0, pwBuf, -1, NULL, NULL, NULL, NULL);

	char* pBuf = new char[nLen + 1];
	memset(pBuf, 0, nLen + 1);

	WideCharToMultiByte(CP_ACP, 0, pwBuf, nwLen, pBuf, nLen, NULL, NULL);

	std::string retStr = pBuf;

	delete[] pBuf;
	delete[] pwBuf;

	pBuf = NULL;
	pwBuf = NULL;

	return retStr;
}

Platform::String^ UTF8_To_Managed_Str(const std::string & str)
{
	int nwLen = MultiByteToWideChar(CP_UTF8, 0, str.c_str(), -1, NULL, 0);

	wchar_t* pwBuf = new wchar_t[nwLen + 1];
	memset(pwBuf, 0, nwLen * 2 + 2);

	MultiByteToWideChar(CP_UTF8, 0, str.c_str(), str.length(), pwBuf, nwLen);

	Platform::String^ pStr = ref new Platform::String(pwBuf);

	delete[] pwBuf;
	pwBuf = NULL;

	return pStr;
}
```