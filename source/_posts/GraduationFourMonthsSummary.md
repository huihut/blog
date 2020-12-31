---

title: 毕业四个月小结
subtitle: 毕业四个月小结
date: 2019-10-30 22:09:00
author: 辉哈
tags:
	- 生活是一首诗
categories: 
	- 生活
	
---


白驹过隙，转眼已经毕业四个月了。

因为工作繁忙（~~变成咸鱼~~），这四个月基本上很少更新博文和 Github。在这里简单总结一下这几个月做了啥吧。

<!-- more -->

![Subway-station-contribution](https://huihut-img.oss-cn-shenzhen.aliyuncs.com/Subway-station-contribution.png)

## 工作上

转正后调组了，业务的技术栈也从 C# / WPF，转到 C++ / Qt，但也还是 Windows 开发。

因为在一个新的组，做的东西也变动很大。七八月搞了一大堆后，就 ~~暂时~~ 不搞了。九月和十月在原有平台使用 CEF 框架接入小程序，至今也在搞这块的业务。

## 商业项目

### Avalive 

[Avalive](https://github.com/avamoe/Avalive) 是从 [Facemoji](https://github.com/huihut/Facemoji) 衍生出来的项目，是一个 Wiondows/MacOS 端的虚拟形象角色扮演的软件。它原本是我的毕业设计，后来去掉一些体验不好的功能，迭代了几个版本，现在准备上架 [Steam](https://store.steampowered.com/app/1137770/Avalive)。

之前做的 Facemoji 是 Android 端的，可以角色扮演、也可以语音聊天，但是从产品角度来说，这两个功能其实本质上是矛盾的。角色扮演是让用户扮演成虚拟形象，把用户的 “思维” 赋予虚拟形象；而语音聊天则是用户与虚拟形象聊天，此时虚拟形象有自己的 “思维”。

Avalive 则是没有语音聊天，只有角色扮演和看板娘。看板娘就如我的博客右下角的猫咪，或者 avamoe 网站（<https://ava.moe>）的虚拟形象差不多，只保留鼠标、语音交互的虚拟形象。

这四个月主要做了 Avalive 的：

* [x] 移除推流、拍照、录像功能，移除 UnityChan 模型
* [x] 移植到 MacOS
* [x] 全球化：简体中文、繁体中文、英文、日文
* [x] 增加几个 Live2d 模型，修改 UI、日志
* [x] 修复一些异常、字符路径、多客户端等问题
* [x] Steam 商店页面文案、图片、视频等制作，Youtube、Github 注册

原本 Avalive 9 月初就能发布的，还能赶上万圣节活动，但是提交给 Steam 没想到官方还要测试，测试到一个问题（有些模型能正常使用摄像头，有些则不行）我这边怎么也复现不了，来回提交了几次，后来只能在商店页面注明可能某些型号摄像头不支持。再提交又因为苹果新的 Mac 应用都需要公证了，要我的应用公证了才能上 Mac 的 Steam（如这个通知 [Steam 上新 macOS 应用程序需求已更新](https://steamcommunity.com/groups/steamworks/announcements/detail/3632639303428097613)）。但是公证应用要苹果开发者账号。噗！

至于 Avalive 的介绍与 Steam 的上架应该还会再写篇博文说说的。（~~又挖坑？~~）

## 其他研究

### DirectX 及 CEF 多显卡支持

这段时间也简单接触了一下 DirectX，主要是 DirectX 11。

[CEF](https://en.wikipedia.org/wiki/Chromium_Embedded_Framework) 多显卡支持的研究是为了解决这两种情况下投屏到预览的问题：

1. 某些显卡可能不支持 CEF 的离屏渲染
2. CEF 进程和投屏到预览的进程不在同一个显卡上运行导致不能共享显存中的纹理

这种时候就需要使用共享内存的方式实现投屏到预览了。

至于 “投屏到预览” 的意思，就如下图所示，把 CEF 小程序的画面（左边），“投屏” 到另一个直播进程的预览图层上（右边）。如果不能投屏，那一块图层就会是全黑的。

这个问题之后可能会写篇博文说说的。（~~又双挖坑？~~）

![](https://huihut-img.oss-cn-shenzhen.aliyuncs.com/20191030171833.png)

### glog 谷歌日志模块

[glog](https://github.com/google/glog) 日志模块，Google 出品，看起来不错，用起来才发现很坑。 

1. 不能把不同模块的日志写在不同文件
2. 日志文件名、每条日志格式不能自定义（如 [can glog write to a specified file name? #147](https://github.com/google/glog/issues/147)）

这两点只有通过改源码才可以实现，而且如果要实现第一点还要自己管理一个日志文件列表。

最终还是使用 glog，改了下源码，写了个恶心的版本（[huihut/glog](https://github.com/huihut/glog)）先用着，然后这个项目就 ~~暂时~~ 不搞了。

### 百度语音合成、讯飞语音识别

百度语音合成是 C# 项目使用，用的是 web 接口：[RestAPI 接口](https://ai.baidu.com/docs#/TTS-API/top)，也额外写了个 Demo：[huihut/BaiduSpeechDemo](https://github.com/huihut/BaiduSpeechDemo)。

讯飞语音识别是 C++ 项目使用，用的是讯飞的 Windows SDK：[语音听写 Windows SDK 文档](https://www.xfyun.cn/doc/asr/voicedictation/Windows-SDK.html)。

## 总结

一篇博文写下来，发现摸鱼的这四个月好像还是能水几篇博文的。哈~

其实这段时间也在笔记中记录自己每天/每周做了什么，也在思考如何提升自己，如何利用已有的资源增加自己额外的收入，也在尝试一些新东西、新模式。

多尝试、多挑战、多分享，希望提升自己的同时也能为别人带来价值。