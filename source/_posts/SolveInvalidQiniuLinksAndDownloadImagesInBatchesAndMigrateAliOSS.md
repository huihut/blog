---

title: 解决七牛云链接失效以及批量下载图片并迁移阿里云 OSS
subtitle: 解决七牛云链接失效以及批量下载图片并迁移阿里云 OSS
date: 2018-11-08 14:28:00
author: huihut
tags:
	- C/C++
	- OSS
categories: 
	- CS
	
---


## 起因

七牛云对测试域名进行回收，而我博文中图片用的链接仍然是测试域名的链接，因此导致部分链接失效，浏览器返回如下错误：

```json
{"error":"no such domain"}
```

<!-- more -->

## 找回图片对象

经过提交工单与七牛云工程师协商，知道了失效的图片并没有丢失，可通过某些方法找回，解决办法如下：

1. 绑定自定义域名：[如何从测试域名过渡到自定义域名](https://developer.qiniu.com/kodo/kb/5158/how-to-transition-from-test-domain-name-to-a-custom-domain-name)（绑定后则可用自定义域名访问失效的图片）
2. 下载单个图片：[命令行辅助工具(qrsctl)](https://developer.qiniu.com/kodo/tools/1300/qrsctl)（使用 qrsctl 工具的 get 下载），如下：
    ```
    .\qrsctl.exe login huihut@outlook.com 123   # 假设密码为 123
    .\qrsctl.exe get temp H.png ./H.png         # 下载 temp 存储空间的 H.png 文件，保存到当前目录下
    ```
3. 下载多个对象
    1. 先新建一个同区域存储空间，会分配一个新的测试域名到新空间
    2. 通过 qshell batchcopy 到有域名的同区域空间然后再进行 qdownload 下载操作：[命令行工具(qshell)](https://developer.qiniu.com/kodo/tools/1302/qshell)
        1. `qshell listbucket <原bucket名> list.txt` （list 出全部文件）  
			<https://github.com/qiniu/qshell/blob/master/docs/listbucket.md>
        2. `cat list.txt | awk '{print $1}' >list_final.txt` （ 用awk获取list结果的第一列）
        3. `qshell batchcopy <原bucket名> <新bucket名> list_final.txt` （复制到新bucket的文件和原bucket文件名一致）  
			<https://github.com/qiniu/qshell/blob/master/docs/batchcopy.md>
        4. `qshell qdownload newfilelist.txt` （newfilelist.txt 为下载的配置文档）  
			<https://github.com/qiniu/qshell/blob/master/docs/qdownload.md>

> [视频教程 -- qshell qrsctl qfetch 命令行工具使用](https://developer.qiniu.com/kodo/kb/3858/video-of-how-to-use-qrs-tools)

## 准备迁移阿里云 OSS

上面的解决方法中，要想继续使用七牛云存储作为图床，必须要有个实名认证的域名（解决方法一），然后绑定之

而我并没有，因此我打算把我的图片对象迁移到 [阿里云 OSS 存储](https://cn.aliyun.com/product/oss)

上们的解决方法二、三都可以，但是二非常麻烦，三需要两个存储空间，也就需要对七牛账号进行实名认证

而我又不想另外实名认证（~~瞎折腾~~），因此我用了第二种方法：下载单个图片！[捂脸]

## C++ 程序下载七牛存储的对象

使用 qrsctl 工具只能单个文件下载，为了避免重复的工作，我写了个 C++ 程序解决。

### 获取包含所有对象名的 html 文件

首先进入七牛云要下载的 bucket（存储空间）的 web 界面，把内容管理列表中全部对象显示出来（点底部加载更多直到全部显示），在 Chrome 浏览器 `右键` - `检查`，如下图，选中 `tbody` - `右键` - `Copy` - `Copy element`

![QiniuStorageSpaceTbody](https://huihut-img.oss-cn-shenzhen.aliyuncs.com/QiniuStorageSpaceTbody.png)

然后把复制到的内容保存成 `html.html` 文件，放到一个目录下，如我的 `D:\code\test\`（以下均以此目录为工程目录）

### 下载并登录 qrsctl

下载 [命令行辅助工具(qrsctl)](https://developer.qiniu.com/kodo/tools/1300/qrsctl)，保存到 `D:\code\test\` 目录，并重命名为 `qrsctl.exe`

在 `D:\code\test\` 目录的 CMD 或 PowerShell 中输入如下命令登录你的七牛账号

```
.\qrsctl.exe login huihut@outlook.com 123   # 假设密码为 123
```

### 编写批量下载程序

在 `D:\code\test\` 下创建个 `QrsctlGet.cpp`，把下面 C++ 代码保存到刚创建的文件，修改存储空间名为你的存储空间名的名字（本文为 `temp`），用 `VS Code` 或其他工具编译运行，七牛存储的文件即会下载到同目录下。

Github 仓库：[huihut/QrsctlGet](https://github.com/huihut/QrsctlGet)（有 `QrsctlGet.Cpp`、`qrsctl.exe`、`.vscode环境`）

```cpp
//==============================================================
//  作者：huihut
//  邮箱：huihut@outlook.com
//  时间：2018-11-08 14:28:00
//  说明：匹配 HTML 的七牛云存储的文件名，并使用 qrsctl 下载文件
//==============================================================

#include <iostream>
#include <string>
#include <sstream>
#include <fstream>
#include <regex>
using namespace std;

int main()
{
	// 存储空间名【需要修改为你的存储空间名】
	string bucket = "temp";

	// 打开 html 文件
	ifstream fhtml;
	fhtml.open("html.html");

	// 统计下载的文件对象数
	int num = 0;

	if (fhtml)
	{
		// tbody 里面符合 reg 的奇数为文件对象，偶数是文件类型，因此只取奇数匹配项
		bool isObj = true;
		std::stringstream buffer;
		buffer << fhtml.rdbuf();
		std::string contents(buffer.str());
		std::smatch match;
		std::regex reg("\\b(edit-word ng-binding\">)([^<]*)");
		while (std::regex_search(contents, match, reg)) {
			if (isObj)
			{
				// 匹配到的文件对象
				string objfile = match.format("$2");
				cout << "Matched " + objfile << endl;

				// 合成下载命令
				string command = "qrsctl.exe get " + bucket + " " + objfile + " ./" + objfile;

				// 下载文件
				cout << "Download " + objfile + "..." << endl;
				system(command.c_str());

				// 文件个数加一
				num++;

				// 下一个非文件对象
				isObj = false;
			}
			else
			{
				// 下一个是文件对象
				isObj = true;
			}
			contents = match.suffix().str();
		}
	}
	else
	{
		cerr << "Failed to read html file!" << endl;
		system("pause");
		return -1;
	}

	cout << "Downloaded " + to_string(num) + " files." << endl;
	system("pause");
	return 0;
}
```

![QiniuQrsctlDownload](https://huihut-img.oss-cn-shenzhen.aliyuncs.com/QiniuQrsctlDownload.png)

## 迁移阿里云 OSS

在 [阿里云 OSS 存储](https://cn.aliyun.com/product/oss) 创建阿里云账号，并在 OSS 中新建 Bucket，其中读写权限选中公共读（public-read），以便能在博文中读取

把上面下载的图片上传到阿里云 OSS，可以使用客户端上传下载：[aliyun/oss-browser](https://github.com/aliyun/oss-browser)

然后获取外链，批量修改我的博文，把所有七牛云链接改为阿里云就好了