---

title: 一个基于 C# 的简单的线程安全日志模块
subtitle: 一个基于 C# 的线程安全日志模块
date: 2019-07-28 14:14:40
author: huihut
tags:
	- Dotnet
	- C#
categories: 
	- CS
	
---

一个基于 C# 的简单的线程安全日志模块，它使用生产者 - 消费者模式，可以在 .NET Framework 和 .Net Core 中使用。

Github 地址：[LogConsumer](https://github.com/huihut/LogConsumer)

<!-- more -->

## 使用

1. 将 [LogConsumer.cs](https://github.com/huihut/LogConsumer/blob/master/LogConsumer/LogConsumer.cs) 添加到你的项目中
2. 将 `LogConsumer.cs` 中的 `logFileName` 修改为你的路径文件名
3. 在需要输出日志的地方使用它
    ```cs
    HuiHut.LogConsumer.LogConsumer.Instance.Write("your log content");
    ```

## 编译运行

* 命令行
    ```
    dotnet build
    dotnet .\LogConsumer\bin\Debug\netcoreapp2.1\LogConsumer.dll
    ```
* Visual Studio  
    1. 打开 `LogConsumer.sln`
    2. 生成解决方案，运行测试

## 测试的日志文件示例

10 个线程，每个线程抛出  10 条日志的测试

```
Data       Time      Namespace          Class           Method    LogContent
----------------------------------------------------------------------------------
2019-05-10 01:50:09  HuiHut.LogConsumer:LogConsumerTest.WriteLog  [ 5 ] index = 0
2019-05-10 01:50:09  HuiHut.LogConsumer:LogConsumerTest.WriteLog  [ 2 ] index = 0
2019-05-10 01:50:09  HuiHut.LogConsumer:LogConsumerTest.WriteLog  [ 7 ] index = 0
2019-05-10 01:50:09  HuiHut.LogConsumer:LogConsumerTest.WriteLog  [ 4 ] index = 0
2019-05-10 01:50:09  HuiHut.LogConsumer:LogConsumerTest.WriteLog  [ 3 ] index = 0
2019-05-10 01:50:09  HuiHut.LogConsumer:LogConsumerTest.WriteLog  [ 6 ] index = 0
2019-05-10 01:50:09  HuiHut.LogConsumer:LogConsumerTest.WriteLog  [ 0 ] index = 0
2019-05-10 01:50:09  HuiHut.LogConsumer:LogConsumerTest.WriteLog  [ 1 ] index = 0
2019-05-10 01:50:09  HuiHut.LogConsumer:LogConsumerTest.WriteLog  [ 9 ] index = 0
2019-05-10 01:50:09  HuiHut.LogConsumer:LogConsumerTest.WriteLog  [ 8 ] index = 0
2019-05-10 01:50:09  HuiHut.LogConsumer:LogConsumerTest.WriteLog  [ 2 ] index = 1
2019-05-10 01:50:09  HuiHut.LogConsumer:LogConsumerTest.WriteLog  [ 7 ] index = 1
2019-05-10 01:50:09  HuiHut.LogConsumer:LogConsumerTest.WriteLog  [ 5 ] index = 1
2019-05-10 01:50:09  HuiHut.LogConsumer:LogConsumerTest.WriteLog  [ 5 ] index = 2
2019-05-10 01:50:09  HuiHut.LogConsumer:LogConsumerTest.WriteLog  [ 3 ] index = 1
2019-05-10 01:50:09  HuiHut.LogConsumer:LogConsumerTest.WriteLog  [ 6 ] index = 1
2019-05-10 01:50:09  HuiHut.LogConsumer:LogConsumerTest.WriteLog  [ 0 ] index = 1
2019-05-10 01:50:09  HuiHut.LogConsumer:LogConsumerTest.WriteLog  [ 1 ] index = 1
2019-05-10 01:50:09  HuiHut.LogConsumer:LogConsumerTest.WriteLog  [ 2 ] index = 2
2019-05-10 01:50:09  HuiHut.LogConsumer:LogConsumerTest.WriteLog  [ 9 ] index = 1
2019-05-10 01:50:09  HuiHut.LogConsumer:LogConsumerTest.WriteLog  [ 5 ] index = 3
2019-05-10 01:50:09  HuiHut.LogConsumer:LogConsumerTest.WriteLog  [ 3 ] index = 2
2019-05-10 01:50:09  HuiHut.LogConsumer:LogConsumerTest.WriteLog  [ 6 ] index = 2
2019-05-10 01:50:09  HuiHut.LogConsumer:LogConsumerTest.WriteLog  [ 7 ] index = 2
2019-05-10 01:50:09  HuiHut.LogConsumer:LogConsumerTest.WriteLog  [ 8 ] index = 1
2019-05-10 01:50:09  HuiHut.LogConsumer:LogConsumerTest.WriteLog  [ 4 ] index = 1
2019-05-10 01:50:09  HuiHut.LogConsumer:LogConsumerTest.WriteLog  [ 0 ] index = 2
2019-05-10 01:50:09  HuiHut.LogConsumer:LogConsumerTest.WriteLog  [ 1 ] index = 2
2019-05-10 01:50:09  HuiHut.LogConsumer:LogConsumerTest.WriteLog  [ 2 ] index = 3
2019-05-10 01:50:09  HuiHut.LogConsumer:LogConsumerTest.WriteLog  [ 9 ] index = 2
2019-05-10 01:50:09  HuiHut.LogConsumer:LogConsumerTest.WriteLog  [ 5 ] index = 4
2019-05-10 01:50:09  HuiHut.LogConsumer:LogConsumerTest.WriteLog  [ 3 ] index = 3
2019-05-10 01:50:09  HuiHut.LogConsumer:LogConsumerTest.WriteLog  [ 6 ] index = 3
2019-05-10 01:50:09  HuiHut.LogConsumer:LogConsumerTest.WriteLog  [ 7 ] index = 3
2019-05-10 01:50:09  HuiHut.LogConsumer:LogConsumerTest.WriteLog  [ 8 ] index = 2
2019-05-10 01:50:09  HuiHut.LogConsumer:LogConsumerTest.WriteLog  [ 4 ] index = 2
2019-05-10 01:50:09  HuiHut.LogConsumer:LogConsumerTest.WriteLog  [ 0 ] index = 3
2019-05-10 01:50:09  HuiHut.LogConsumer:LogConsumerTest.WriteLog  [ 1 ] index = 3
2019-05-10 01:50:09  HuiHut.LogConsumer:LogConsumerTest.WriteLog  [ 2 ] index = 4
2019-05-10 01:50:09  HuiHut.LogConsumer:LogConsumerTest.WriteLog  [ 9 ] index = 3
2019-05-10 01:50:09  HuiHut.LogConsumer:LogConsumerTest.WriteLog  [ 5 ] index = 5
2019-05-10 01:50:09  HuiHut.LogConsumer:LogConsumerTest.WriteLog  [ 3 ] index = 4
2019-05-10 01:50:09  HuiHut.LogConsumer:LogConsumerTest.WriteLog  [ 6 ] index = 4
2019-05-10 01:50:09  HuiHut.LogConsumer:LogConsumerTest.WriteLog  [ 7 ] index = 4
2019-05-10 01:50:09  HuiHut.LogConsumer:LogConsumerTest.WriteLog  [ 8 ] index = 3
2019-05-10 01:50:09  HuiHut.LogConsumer:LogConsumerTest.WriteLog  [ 4 ] index = 3
2019-05-10 01:50:09  HuiHut.LogConsumer:LogConsumerTest.WriteLog  [ 0 ] index = 4
2019-05-10 01:50:09  HuiHut.LogConsumer:LogConsumerTest.WriteLog  [ 1 ] index = 4
2019-05-10 01:50:09  HuiHut.LogConsumer:LogConsumerTest.WriteLog  [ 2 ] index = 5
2019-05-10 01:50:09  HuiHut.LogConsumer:LogConsumerTest.WriteLog  [ 9 ] index = 4
2019-05-10 01:50:09  HuiHut.LogConsumer:LogConsumerTest.WriteLog  [ 5 ] index = 6
2019-05-10 01:50:09  HuiHut.LogConsumer:LogConsumerTest.WriteLog  [ 3 ] index = 5
2019-05-10 01:50:09  HuiHut.LogConsumer:LogConsumerTest.WriteLog  [ 6 ] index = 5
2019-05-10 01:50:09  HuiHut.LogConsumer:LogConsumerTest.WriteLog  [ 7 ] index = 5
2019-05-10 01:50:09  HuiHut.LogConsumer:LogConsumerTest.WriteLog  [ 8 ] index = 4
2019-05-10 01:50:09  HuiHut.LogConsumer:LogConsumerTest.WriteLog  [ 4 ] index = 4
2019-05-10 01:50:09  HuiHut.LogConsumer:LogConsumerTest.WriteLog  [ 0 ] index = 5
2019-05-10 01:50:09  HuiHut.LogConsumer:LogConsumerTest.WriteLog  [ 1 ] index = 5
2019-05-10 01:50:09  HuiHut.LogConsumer:LogConsumerTest.WriteLog  [ 2 ] index = 6
2019-05-10 01:50:09  HuiHut.LogConsumer:LogConsumerTest.WriteLog  [ 9 ] index = 5
2019-05-10 01:50:09  HuiHut.LogConsumer:LogConsumerTest.WriteLog  [ 5 ] index = 7
2019-05-10 01:50:09  HuiHut.LogConsumer:LogConsumerTest.WriteLog  [ 3 ] index = 6
2019-05-10 01:50:09  HuiHut.LogConsumer:LogConsumerTest.WriteLog  [ 6 ] index = 6
2019-05-10 01:50:09  HuiHut.LogConsumer:LogConsumerTest.WriteLog  [ 7 ] index = 6
2019-05-10 01:50:09  HuiHut.LogConsumer:LogConsumerTest.WriteLog  [ 8 ] index = 5
2019-05-10 01:50:09  HuiHut.LogConsumer:LogConsumerTest.WriteLog  [ 4 ] index = 5
2019-05-10 01:50:09  HuiHut.LogConsumer:LogConsumerTest.WriteLog  [ 0 ] index = 6
2019-05-10 01:50:09  HuiHut.LogConsumer:LogConsumerTest.WriteLog  [ 1 ] index = 6
2019-05-10 01:50:09  HuiHut.LogConsumer:LogConsumerTest.WriteLog  [ 2 ] index = 7
2019-05-10 01:50:09  HuiHut.LogConsumer:LogConsumerTest.WriteLog  [ 9 ] index = 6
2019-05-10 01:50:09  HuiHut.LogConsumer:LogConsumerTest.WriteLog  [ 5 ] index = 8
2019-05-10 01:50:09  HuiHut.LogConsumer:LogConsumerTest.WriteLog  [ 3 ] index = 7
2019-05-10 01:50:09  HuiHut.LogConsumer:LogConsumerTest.WriteLog  [ 6 ] index = 7
2019-05-10 01:50:09  HuiHut.LogConsumer:LogConsumerTest.WriteLog  [ 7 ] index = 7
2019-05-10 01:50:09  HuiHut.LogConsumer:LogConsumerTest.WriteLog  [ 8 ] index = 6
2019-05-10 01:50:09  HuiHut.LogConsumer:LogConsumerTest.WriteLog  [ 4 ] index = 6
2019-05-10 01:50:09  HuiHut.LogConsumer:LogConsumerTest.WriteLog  [ 0 ] index = 7
2019-05-10 01:50:09  HuiHut.LogConsumer:LogConsumerTest.WriteLog  [ 1 ] index = 7
2019-05-10 01:50:09  HuiHut.LogConsumer:LogConsumerTest.WriteLog  [ 2 ] index = 8
2019-05-10 01:50:09  HuiHut.LogConsumer:LogConsumerTest.WriteLog  [ 9 ] index = 7
2019-05-10 01:50:09  HuiHut.LogConsumer:LogConsumerTest.WriteLog  [ 5 ] index = 9
2019-05-10 01:50:09  HuiHut.LogConsumer:LogConsumerTest.WriteLog  [ 3 ] index = 8
2019-05-10 01:50:09  HuiHut.LogConsumer:LogConsumerTest.WriteLog  [ 6 ] index = 8
2019-05-10 01:50:09  HuiHut.LogConsumer:LogConsumerTest.WriteLog  [ 7 ] index = 8
2019-05-10 01:50:09  HuiHut.LogConsumer:LogConsumerTest.WriteLog  [ 8 ] index = 7
2019-05-10 01:50:09  HuiHut.LogConsumer:LogConsumerTest.WriteLog  [ 4 ] index = 7
2019-05-10 01:50:09  HuiHut.LogConsumer:LogConsumerTest.WriteLog  [ 0 ] index = 8
2019-05-10 01:50:09  HuiHut.LogConsumer:LogConsumerTest.WriteLog  [ 1 ] index = 8
2019-05-10 01:50:09  HuiHut.LogConsumer:LogConsumerTest.WriteLog  [ 2 ] index = 9
2019-05-10 01:50:09  HuiHut.LogConsumer:LogConsumerTest.WriteLog  [ 9 ] index = 8
2019-05-10 01:50:09  HuiHut.LogConsumer:LogConsumerTest.WriteLog  [ 5 ] Thread is finished.
2019-05-10 01:50:09  HuiHut.LogConsumer:LogConsumerTest.WriteLog  [ 3 ] index = 9
2019-05-10 01:50:09  HuiHut.LogConsumer:LogConsumerTest.WriteLog  [ 6 ] index = 9
2019-05-10 01:50:09  HuiHut.LogConsumer:LogConsumerTest.WriteLog  [ 7 ] index = 9
2019-05-10 01:50:09  HuiHut.LogConsumer:LogConsumerTest.WriteLog  [ 8 ] index = 8
2019-05-10 01:50:09  HuiHut.LogConsumer:LogConsumerTest.WriteLog  [ 4 ] index = 8
2019-05-10 01:50:09  HuiHut.LogConsumer:LogConsumerTest.WriteLog  [ 0 ] index = 9
2019-05-10 01:50:09  HuiHut.LogConsumer:LogConsumerTest.WriteLog  [ 1 ] index = 9
2019-05-10 01:50:09  HuiHut.LogConsumer:LogConsumerTest.WriteLog  [ 2 ] Thread is finished.
2019-05-10 01:50:09  HuiHut.LogConsumer:LogConsumerTest.WriteLog  [ 9 ] index = 9
2019-05-10 01:50:09  HuiHut.LogConsumer:LogConsumerTest.WriteLog  [ 3 ] Thread is finished.
2019-05-10 01:50:09  HuiHut.LogConsumer:LogConsumerTest.WriteLog  [ 6 ] Thread is finished.
2019-05-10 01:50:09  HuiHut.LogConsumer:LogConsumerTest.WriteLog  [ 7 ] Thread is finished.
2019-05-10 01:50:09  HuiHut.LogConsumer:LogConsumerTest.WriteLog  [ 8 ] index = 9
2019-05-10 01:50:09  HuiHut.LogConsumer:LogConsumerTest.WriteLog  [ 4 ] index = 9
2019-05-10 01:50:09  HuiHut.LogConsumer:LogConsumerTest.WriteLog  [ 0 ] Thread is finished.
2019-05-10 01:50:09  HuiHut.LogConsumer:LogConsumerTest.WriteLog  [ 1 ] Thread is finished.
2019-05-10 01:50:09  HuiHut.LogConsumer:LogConsumerTest.WriteLog  [ 9 ] Thread is finished.
2019-05-10 01:50:09  HuiHut.LogConsumer:LogConsumerTest.WriteLog  [ 8 ] Thread is finished.
2019-05-10 01:50:09  HuiHut.LogConsumer:LogConsumerTest.WriteLog  [ 4 ] Thread is finished.
```