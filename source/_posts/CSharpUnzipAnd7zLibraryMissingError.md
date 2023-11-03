---

title: C# 解压压缩包及 7z 库缺失导致 Can not load 7-zip library or internal COM error!
subtitle: C# 解压压缩包及 7z 库缺失导致 Can not load 7-zip library or internal COM error!
date: 2018-11-07 00:20:00
author: huihut
tags:
	- Dotnet
	- C#
categories: 
	- CS
	
---

## 表现

C# 程序解压 7z 文件的时候抛出异常

```
Can not load 7-zip library or internal COM error! Message: DLL file does not exist.
```

<!-- more -->

## 原因

程序无法找到 7z 库，导致无法解压 7z 文件

## 解决

### 方法一：使用 NuGet 包管理器下载安装

选择 Visual Studio 某个项目 - 右键引用 - 管理 NuGet 程序包 - 浏览 - 搜索 `7z`，选择一个 `7z` 包，如 `7z.Libs` 安装即可。

### 方法二：手动下载安装

在官网 [Download 7-Zip ](https://www.7-zip.org/download.html) 下载下面的 7z 库

Link|Type|Windows|Description
---|---|---|---
[Download](https://www.7-zip.org/a/7z1805-extra.7z) | .7z | x86 / x64 | 7-Zip Extra: standalone console version, 7z DLL, Plugin for Far Manager

解压重命名到 `C:\Program Files\7-Zip`

## 使用

```cs
/// <summary>
/// 解压压缩包
/// </summary>
/// <param name="file_path">压缩包路径</param>
/// <param name="save_path">解压后保存路径</param>
/// <returns>是否解压成功</returns>
private bool UncompressFile(string file_path, string save_path)
{
    try
    {
        if (System.IO.Directory.Exists(save_path))
        {
            System.IO.Directory.Delete(save_path, true);
        }
        // 若手动安装，需要指定路径
        //SevenZip.SevenZipExtractor.SetLibraryPath(@"C:\Program Files\7-Zip\7za.dll");
        SevenZip.SevenZipExtractor extractor = new SevenZip.SevenZipExtractor(file_path);
        extractor.ExtractArchive(save_path);
        extractor.Dispose();
    }
    catch (Exception e1)
    {
        System.Diagnostics.Debug.WriteLine(e1.Message);
        return false;
    }
    return true;
}
```