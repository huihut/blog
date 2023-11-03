---

title: 一个可以模仿你的表情的语音聊天机器人 —— Facemoji 废萌（OpenCV+Dlib+Live2D+图灵机器人+讯飞IAT语音听写+讯飞TTS语音合成）
subtitle: 一个可以模仿你的表情的语音聊天机器人 —— Facemoji 废萌（OpenCV+Dlib+Live2D+图灵机器人+讯飞IAT语音听写+讯飞TTS语音合成）
date: 2018-02-08 00:11:00
author: huihut
tags:
	- Unity
categories: 
	- CS
	
---

## 概述

本文介绍一个可以模仿你的表情的语音聊天机器人 —— Facemoji 废萌

这是个 Unity 项目，其暂时有两个模块 ：

* 【模块一】是实时人脸卡通化（FaceTracking），使用 [OpenCV](https://enoxsoftware.com/opencvforunity/) 和 [Dlib](https://enoxsoftware.com/dlibfacelandmarkdetector/) 检测面部表情，并实时转化为 [Live2D](http://sites.cybernoids.jp/cubism-sdk2_e/unity_2-1) 模型，然后可 [录制](https://github.com/Chman/Moments) 成 gif 图；
* 【模块二】是人工智能（AI）使用 [图灵机器人](http://www.tuling123.com/)、[讯飞IAT语音听写](http://www.xfyun.cn/services/voicedictation)、[讯飞TTS语音合成](http://www.xfyun.cn/services/online_tts) 进行语音聊天。

<!-- more -->

## 预览

![Facemoji效果图_cn](http://huihut-img.oss-cn-shenzhen.aliyuncs.com/Facemoji%E6%95%88%E6%9E%9C%E5%9B%BE_cn.png)

## 源码

源码：[Github . huihut/Facemoji](https://github.com/huihut/Facemoji)

## 下载

* [酷安 . Facemoji 废萌](https://www.coolapk.com/apk/192260)
* [Google Play (需要梯子)](https://play.google.com/store/apps/details?id=com.huihut.facemoji)
* [Github . Facemoji/releases](https://github.com/huihut/Facemoji/releases)
* [Google云盘 . Facemoji/Platform](https://drive.google.com/open?id=1ofJMFIdzXCdYYO3qO5hvrTQPJUumgSY-)
* [Baidu网盘 . Facemoji/Platform](https://pan.baidu.com/s/1U08B_wPY67Zh1RTwFhrihA)

## 为什么叫废萌（Facemoji）

首先，为什么叫废萌（Facemoji）？...emmm...这个项目其实是由 Animoji 启发的，由于当时 Animoji 没有开放 api，所以想开发个跨平台（Unity）的类似的项目，就叫 Facemoji。

至于中文名废萌嘛？...emmm...总觉得她除了卖萌没什么作用，所以就叫废（Face）萌（Moji）了。

## 制作

1. 从 [Google云盘](https://drive.google.com/open?id=1ofJMFIdzXCdYYO3qO5hvrTQPJUumgSY-) 或者 [Baidu网盘](https://pan.baidu.com/s/1U08B_wPY67Zh1RTwFhrihA)下载`shape_predictor_68_face_landmarks.dat`（已训练好的人脸检测模型）和 `Facemoji_Plugins_Assets_1.5.0.unitypackage`（精简的 OpenCV, Dlib, Live2D 和 Iflytek 库）

2. 克隆下源码：`git clone git@github.com:huihut/Facemoji.git`

3. 创建一个新的 Unity 项目，命名为 Facemoji

4. 把 `Facemoji-master` 文件夹中的 `Assets` 和 `ProjectSettings` 替换 `Facemoji` 的同名文件夹

5. 把 `shape_predictor_68_face_landmarks.dat` 复制到 `Facemoji/Assets/StreamingAssets/`

6. 导入 `Facemoji_Plugins_Assets_1.5.0.unitypackage`。导入后的文件结构如下：  
    ![FacemojiDirectoryStructure1.5.0.png](http://huihut-img.oss-cn-shenzhen.aliyuncs.com/FacemojiDirectoryStructure1.5.0.png)

7. 转换平台到 Android（其他平台未适配）

8. Build & Run

## 使用

### 实时人脸卡通化（FaceTracking）

使用 OpenCV 和 Dlib 检测面部表情，并实时转化为 Live2D 模型；

她可以跟着你的头部表情动，试着摇头看看吧。

### 录制 gif 图

点击顶部中间的录制键可以录制 3 秒的 gif；

录制状态为：Recording（准备录制）、PreProcessing（正在录制）、Paused（正在压缩成gif图）

生成的 gif 存储在 `Application.dataPath`，Android 平台的话在 
`/storage/emulated/0/Android/data/com.huihut.facemoji/files/`

### 语音和文字聊天（~~聊天机器人？~~ 她说她是 AI，不是机器人！ hhhh...）

使用 **图灵机器人**、**讯飞 IAT 语音听写**、**讯飞 TTS 语音合成**

她很智能（~~zhizhang~~），可以：

* 聊天对话
* 生活百科
* 数学计算
* 故事大全
* 笑话大全
* 成语接龙
* 星座运势
* 天气查询
* ...

但是由于她是个中国 AI（~~机器人~~），图灵机器人只支持中文，所以她只能进行中文聊天，和她讲英文她只会翻译。

不过她以后会学习英文的（~~换个会讲英文的~~）。

## Gif演示

![GifCapture-201802072218435427.gif](http://huihut-img.oss-cn-shenzhen.aliyuncs.com/GifCapture-201802072218435427.gif)    

## 异常问题

### 人不动时模型抖动问题（特别是眼睛眉毛）[Facemoji/issues/1](https://github.com/huihut/Facemoji/issues/1)

#### 原因

* 手/眼/眉等的轻微抖动
* Dlib 检测到的每一帧人脸特征点的位置会有轻微差距

#### 解决

* 调整模型参数

    如果人眼睛小的话，可以修改 `FaceTracking.cs` 中 [live2DModelUpdate](https://github.com/huihut/Facemoji/blob/v1.4.1/Assets/Scripts/FaceTracking.cs#L294) 方法，把睁开眼的区间改大一点，如下：

```
// eye_open_L                               // 左眼
float eyeOpen_L = getRaitoOfEyeOpen_L (points);
if (eyeOpen_L > 0.4f && eyeOpen_L < 1.0f)
    eyeOpen_L = 1;                          // 睁开眼
else if (eyeOpen_L >= 1.0f)
    eyeOpen_L = 2;                          // 睁大眼
else if (eyeOpen_L <= 0.4f)
    eyeOpen_L = 0;                          // 闭上眼
live2DModel.PARAM_EYE_L_OPEN = eyeOpen_L;
```

* 考虑使用卡尔曼滤波器或者其他滤波器过滤（未尝试）

## 修改说明

> 2018年03月23日因项目更新 v1.5.0 版本而对此博文做了修改。因博文内容不常改动，项目部署、文档等以Github为准。