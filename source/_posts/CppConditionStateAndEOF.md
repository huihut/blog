---

title: C++ IO 库条件状态及EOF
subtitle: C++ 文件结束符 EOF
date: 2017-04-15 12:12:24
author: huihut
tags:
	- C/C++
categories: 
	- CS
	
---


## 文本文件判空问题

对于空文本文件的判断问题引起了我对 IO 库中条件状态和 EOF 的探究。

就是这段程序：

	int vehicle_number;
	fstream infile;
    infile.open("vehicle.txt", ios::in);
    if(infile.is_open())
    {
        while(!infile.eof())
        {
        	infile >> vehicle_number;
        	......
        
        }
        infile.close();
    }
    
它可以打开空文本文件并运行到 `while(!infile.eof())` 循环里面。由于是空文本文件，它进入里面会造成一些问题，所以需要对文件进行判空。

<!-- more -->

## EOF

EOF（End Of File） 常用于：

* 文件结束标志
* 函数出错的返回值

在 C++ IO 库中可用作：

* 条件状态的判定

现在让我们看看 IO 库中的描述。


## IO 库条件状态

【表一】IO 库条件状态

状态 | 含义
--- | --- 
strm::iostate | strm是一种IO类型。iostream是一种机器相关的整型提供了表达条件状态的完整功能
strm::badbit | strm::badbit用来指出流已崩溃
strm::failbit | strm::failbit用来指出一个IO操作失败了
strm::eofbit | strm::eofbit用来指出流达到了文件结束
strm::goodbit | strm::goodbit用来指出流未处于错误状态。此值保证为零
s.eof() | 流 s 的 eofbit 置位，则返回 true
s.fail()| 流 s 的 failbit 或 badbit 置位，则返回 true
s.bad()| 流 s 的 badbit 置位，则返回 true
s.good()| 若流 s 处于有效状态，则返回 true
s.clear()|将流 s 中的所有条件状态位复位，将流的状态设置为有效。返回void
s.clear(flag)| 根据给定的 flags 标志位，将流 s 中对于条件状态位复位。 flag 的类型是strm::iostate。返回 void
s.setstate(flag)| 根据给定的 flags 标志位，将流 s 中对于条件状态位置位。 flag 的类型是strm::iostate。返回 void
s.rdstate()|返回流 s 的当前条件，返回值类型为 strm::iostate

【表二】四种条件状态

状态 | 含义 | 数值 | good() | eof() | bad() |fail()|rdstate()
---|---|---|---|---|---|---|---
ios::goodbit|流状态完全正常|0|1|0|0|0|goodbit
ios::eofbit|已达到文件结束|2|0|1|0|0|eofbit
ios::badbit	|输入（输出）流出现非致命错误，可挽回|1|0|0|1|0|badbit
ios::failbit|输入（输出）流出现致命错误，不可挽回|4|0|0|0|1|failbit

我们先看【表一】的关于 EOF 的这两行：

状态 | 解释
--- | --- 
strm::eofbit | strm::eofbit用来指出流达到了文件结束
s.eof() | 流 s 的 eofbit 置位，则返回 true

这里指出当流读到文件结束时，`eofbit` 置 `1` ，用于标识读取到文件的末尾。

而 `eof()` 是当 `eofbit` 置位（置 `1`）时才返回，并不是读到文件末尾就返回。

**所以使用 `eof()` 读取文件，读到结束标志 `EOF` 时不会立刻返回 `true`，只是 `eofbit` 置位，下次调用 `eof()` 才返回 `true`。**

## 文本文件判空问题的解释

现在就可以解释最初的问题了，让我们回过头看看。

当程序第一次运行到 `while(!infile.eof())` 时，`infile.eof()` 读到文件末尾的 `EOF`，但并不是立刻返回 `true`，只是 `eofbit` 置位，所以 `infile.eof()` 还是 `false` 的状态，所以会进入 `while` 循环。

## 文本文件判空问题的解决

* 把流对象状态当做条件使用

		if(infile.is_open())
    	{
        	while(infile >> vehicle_number)
        	{
            	......
        	}
    	}
    
* 使用 `peek()`

	`istream::peek()` 用于读取并返回流的下一个字符（返回值为 `char` 类型），但并不读取该字符到输入流中，即流指针依然指向原来位置，并不后移。
	
		if(infile.is_open())
    	{
       		while(infile.peek() != EOF)
        	{
            	infile >> vehicle_number;
            	......
        	}
    	}
    	

## 重复读入非空文本文件最后一个字符问题

经过上面的解释，我们已经知道了文本文件如何判空。但由于 EOF 的锅，若用 `while(!infile.eof())` 还会导致重复读入非空文本文件最后一个字符的问题。

看下面这段代码：

	char c;
    fstream infile;
    infile.open("test.txt", ios::in);

    if(infile.is_open())
    {
        while(!infile.eof())
        {
            infile >> c;
            cout << c;
        }
    }
    infile.close();
    
和文本文件判空问题的代码相似，只是读入字符存储到 `char` 类型变量中，并把其输出。

然后我们在 `test.txt` 中保存 `abc` 这三个字符。

运行的结果是：

	abcc

即 `while(!infile.eof())` 重复执行了最后一趟，多输入了字符 `c` 。

这个问题和文本文件判空问题的解决办法一样，都是使用 `peek()` 或者把流对象当做 `while` 的条件来解决。

## 总结

* 使用 `eof()` 读取文件，读到结束标志 `EOF` 时不会立刻返回 `true`，只是 `eofbit` 置位，下次调用 `eof()` 才返回 `true`。
* 只有一个流处于无错状态时，我们才可以对它读写数据。因此代码通常应该在使用一个流之前检查它是否处于良好状态。
