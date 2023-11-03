---

title: GitHub 弃用 TLS 1.0、1.1 导致 push 异常 SSL routines:SSL23_GET_SERVER_HELLO:tlsv1 alert protocol version
subtitle: GitHub 弃用 TLS 1.0、1.1 导致 push 异常 SSL routines:SSL23_GET_SERVER_HELLO:tlsv1 alert protocol version
date: 2018-02-28 19:13:09
author: huihut
tags:
	- 版本控制
categories: 
	- CS
	
---

## 报错

git push 到 Github 的时候出现异常：

```
fatal: unable to access 'https://github.com/huihut/interview.git/': error:1407742E:SSL routines:SSL23_GET_SERVER_HELLO:tlsv1 alert protocol version
Pushing to https://github.com/huihut/interview.git
```

<!-- more -->

## 原因

在 2018年2月22日19:00 UTC（太平洋标准时间上午11:00），GitHub 停止了对加密弱 TLS 1.0 和 1.1 协议的支持，并且仅支持与 TLS 1.2 协议的连接。

官方声明：

[Github . Weak cryptographic standards removal notice](https://githubengineering.com/crypto-removal-notice/)

因此，如果你的 git 连接方式仍然是 TLS 1.0 或 1.1，则会报错。

## 解决

①  查看你的 TLS 版本

```
git config --global --list
```

②  如果仍然是 TLS 1.0 或 1.1，则下载更新 Git 最新版：<https://git-scm.com/>

③  安装好最新版 Git 后，更新 TLS

```
git config --global --unset http.sslVersion
git config --global --add http.sslVersion tlsv1.2
```

④  现在则可以使用 TLS 1.2 传输，并解决了此问题

操作如下图：

![](http://huihut-img.oss-cn-shenzhen.aliyuncs.com/gitconfighttp.sslVersion.png)