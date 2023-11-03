---

title: Unity3D Plugin xxx is used from several locations 错误
subtitle: Unity3D Plugin 'opencvforunity.dll' is used from several locations 错误
date: 2019-01-07 21:41:35
author: huihut
tags:
	- Unity
categories: 
	- CS
	
---

## 错误

```
Plugin 'opencvforunity.dll' is used from several locations:
 Assets/OpenCVForUnity/Plugins/x86/opencvforunity.dll would be copied to <PluginPath>/opencvforunity.dll
 Assets/OpenCVForUnity/Plugins/x86_64/opencvforunity.dll would be copied to <PluginPath>/opencvforunity.dll
Please fix plugin settings and try again.

UnityEditor.Modules.DefaultPluginImporterExtension:CheckFileCollisions(String)
UnityEditorInternal.PluginsHelper:CheckFileCollisions(BuildTarget) (at D:/unity/Editor/Mono/Plugins/PluginsHelper.cs:25)
UnityEditor.BuildPlayerWindow:BuildPlayerAndRun()
```

<!-- more -->

## 原因

我使用的 `opencvforunity.dll` 库的 32 位版本与 64 位版本在 Unity 编辑器中没有明确指定，所以 Unity 不知道 32 位或者 64 位系统下用哪个 `opencvforunity.dll` 库。

## 解决

* 为 x86 的 `opencvforunity.dll` 取消 x86_64 平台，只勾选 x86 平台，Apply
* 为 x86_64 的 `opencvforunity.dll` 取消 x86 平台，只勾选 x86_64 平台，Apply

![Unity3DOpencvforunity.dll](https://huihut-img.oss-cn-shenzhen.aliyuncs.com/Unity3DOpencvforunity.dll.png)