---

title: Windows 下源码编译 eos 人脸模型拟合库过程遇到的坑坑坑
subtitle: Windows 下源码编译 eos 人脸模型拟合库过程遇到的坑坑坑
date: 2019-01-13 02:09:21
author: huihut
tags:
	- 链接装载库
categories: 
	- CS
	
---


[eos](https://github.com/patrikhuber/eos) 是一个用现代 C++11/14 编写的轻量级三维形变人脸模型拟合库，下面介绍下编译它的步骤和遇到的一些坑坑坑坑！

* 博文的前半部分是用手动安装的 opencv 和 boost 构建和编译的，一路是坑，最终没有编译成功；
* 博文的后半部分是用 [vcpkg](https://github.com/Microsoft/vcpkg/) 构建系统安装的 opencv 和 boost 然后构建和编译的，最终编译成功，但是运行失败。

至今为止，关于运行失败的 [issue](https://github.com/patrikhuber/eos/issues/245) 作者也没有回复，希望有朝一日能填坑吧~

<!-- more -->

## 初期编译环境

* Windows 10
* Visual Studio 2017
* CMake 3.12.4
* OpenCV 4.0.1
* Boost 1.66.0

## 克隆配置

```
git clone --recursive git@github.com:patrikhuber/eos.git
mkdir eos_build
```

打开 CMake-gui，设置

* Where is the source code: `D:/code/VS/BabyCreator/eos`
* Where to build the binaries: `D:/code/VS/BabyCreator/eos_build`

点击 Configure，设置编译器

* Specify the generator for this project: Visual Studio 15 2017
* Finish

配置过程遇到如下错误。

## Found OpenCV Windows Pack but it has no binaries compatible with your configuration. 错误

### 问题

```
  Found OpenCV Windows Pack but it has no binaries compatible with your
  configuration.

  You should manually point CMake variable OpenCV_DIR to your build of OpenCV
  library.
```

![20181229012607](https://huihut-img.oss-cn-shenzhen.aliyuncs.com/20181229012607.png)

### 原因

能找到 OpenCV 路径，但找不到兼容的二进制文件。

出错的这个 OpenCV 是在 [opencv.org/releases](https://opencv.org/releases.html) 下载的 Win pack。

尝试过网友的解决方案【尝试二、三、四】都不能解决，最终通过【尝试五】重新下载源码编译解决，因此可能是构建环境或者编译器的问题。

### 解决

【尝试一】重新设置环境变量：`E:\opencv-4.0.1\opencv-4.0.1-winpack\opencv\build\x64\vc15\bin`

【尝试二】在 `eos/CMakeCache.txt` 设置 OpenCV_DIR，不行。

```
set(OpenCV_DIR "D:\\code\\VS\\BabyCreator\\eos\\3rdparty\\opencv\\build")
find_package(OpenCV REQUIRED)
```

【尝试三】在终端设置 OpenCV_DIR，不行。

```
PS D:\code\VS\BabyCreator\eos> cmake . -DOpenCV_DIR=D:\\code\\VS\\BabyCreator\\eos\\3rdparty\\opencv\\build\\OpenCVConfig.cmake
```

【尝试四】在 find_package 前设置 OpenCV_FOUND 为 1，不行。

```
set(OpenCV_FOUND 1)
find_package(OpenCV REQUIRED)
```

【尝试五】下载源码编译 VS 2017 工程的 opencv，解决。

1. 在 CMake Configure、Generate
2. 在 VS ALL_BUILD、INSTALL
3. 执行 `install/setup_vars_opencv4.cmd`
4. 设置环境变量：`E:/opencv-4.0.1/opencv-4.0.1-vs-build/install/x86/vc15/bin`

##  Imported targets and dependency information not available for Boost version 错误

```
CMake Warning at E:/CMake/share/cmake-3.12/Modules/FindBoost.cmake:577 (message):
  Imported targets and dependency information not available for Boost version
  (all versions older than 1.33)
Call Stack (most recent call first):
  E:/CMake/share/cmake-3.12/Modules/FindBoost.cmake:963 (_Boost_COMPONENT_DEPENDENCIES)
  E:/CMake/share/cmake-3.12/Modules/FindBoost.cmake:1622 (_Boost_MISSING_DEPENDENCIES)
  examples/CMakeLists.txt:22 (find_package)
......
Boost found at Boost_INCLUDE_DIR-NOTFOUND
CMake Error: The following variables are used in this project, but they are set to NOTFOUND.
Please set them or make sure they are set and tested correctly in the CMake files:
D:/code/VS/BabyCreator/eos/examples/Boost_INCLUDE_DIR
   used as include directory in directory D:/code/VS/BabyCreator/eos/examples
   used as include directory in directory D:/code/VS/BabyCreator/eos/examples
   used as include directory in directory D:/code/VS/BabyCreator/eos/examples
   used as include directory in directory D:/code/VS/BabyCreator/eos/examples

Configuring incomplete, errors occurred!
```

![2018-12-29_015957](https://huihut-img.oss-cn-shenzhen.aliyuncs.com/2018-12-29_015957.png)

### 原因

没找到 Boost 库，原因是没配置好。

### 解决

尝试了如下方式后，这个问题变成了 `Unable to find the requested Boost libraries.`
1. 下载 [boost](https://www.boost.org/users/download/) 
2. 运行 `boost_1_66_0/b2.exe` 生成静态库
3. 在 `eos/CMakeCache.txt` 配置
    ```
    set(BOOST_ROOT "E:\\boost_1_66_0")
    set(BOOST_LIBRARYDIR ${BOOST_ROOT}/stage/lib)
    ```

## Unable to find the requested Boost libraries. 错误

```
CMake Error at E:/CMake/share/cmake-3.12/Modules/FindBoost.cmake:2048 (message):
  Unable to find the requested Boost libraries.

  Boost version: 1.66.0

  Boost include path: E:/boost_1_66_0

  Could not find the following Boost libraries:

          boost_system
          boost_filesystem
          boost_program_options

  No Boost libraries were found.  You may need to set BOOST_LIBRARYDIR to the
  directory containing Boost libraries or BOOST_ROOT to the location of
  Boost.
Call Stack (most recent call first):
  examples/CMakeLists.txt:22 (find_package)


Boost found at E:/boost_1_66_0
Configuring incomplete, errors occurred!
```

![2018-12-29_020403](https://huihut-img.oss-cn-shenzhen.aliyuncs.com/2018-12-29_020403.png)

### 原因

找到了 Boost 库，但是找不到 `boost_system`、`boost_filesystem`、`boost_program_options` 这三个库。

### 解决

这个问题太坑了，最终是把 `E:\boost_1_66_0\stage\lib` 里面的那三个库（每个有四个相关库）的名字分别删掉 `lib`，如下对 `boost_system` 的修改：

* `libboost_system-vc141-mt-x32-1_66.lib` 改为 `boost_system-vc141-mt-x32-1_66.lib`
* `libboost_system-vc141-mt-gd-x32-1_66.lib` 改为 `boost_system-vc141-mt-gd-x32-1_66.lib`
* `libboost_system-vc141-mt-x64-1_66.lib` 改为 `boost_system-vc141-mt-x64-1_66.lib`
* `libboost_system-vc141-mt-gd-x64-1_66.lib` 改为 `boost_system-vc141-mt-gd-x64-1_66.lib`

现在终于可以生成工程文件了！

然而 VS 打开工程后编译，又有问题。

## error C2065: “CV_BGR2BGRA”: 未声明的标识符

generate-obj.vcxproj
```
eos\include\eos\render\texture.hpp(101): error C2065: “CV_BGR2BGRA”: 未声明的标识符 (undeclared identifier)
```

### 解决

这个问题可以根据这个 [issue](https://github.com/patrikhuber/4dface/issues/40)，把 `CV_BGR2BGRA` 改为 `cv::COLOR_BGR2BGRA`，或者改为 `0` 解决。

## error LNK2005: xxx 已经在 xxx 中定义；error LNK2019: 无法解析的外部符号

fit-model-simple.vcxproj
```
1>boost_program_options-vc140-mt-gd.lib(boost_program_options-vc141-mt-gd-x32-1_68.dll) : error LNK2005: "public: __thiscall boost::program_options::value_semantic_codecvt_helper<char>::value_semantic_codecvt_helper<char>(void)" (??0?$value_semantic_codecvt_helper@D@program_options@boost@@QAE@XZ) 已经在 boost_program_options-vc141-mt-gd-x32-1_66.lib(value_semantic.obj) 中定义
1>boost_program_options-vc140-mt-gd.lib(boost_program_options-vc141-mt-gd-x32-1_68.dll) : error LNK2005: "public: virtual __thiscall boost::program_options::value_semantic_codecvt_helper<char>::~value_semantic_codecvt_helper<char>(void)" (??1?$value_semantic_codecvt_helper@D@program_options@boost@@UAE@XZ) 已经在 boost_program_options-vc141-mt-gd-x32-1_66.lib(value_semantic.obj) 中定义
1>boost_program_options-vc140-mt-gd.lib(boost_program_options-vc141-mt-gd-x32-1_68.dll) : error LNK2005: "void __cdecl boost::program_options::validate(class boost::any &,class std::vector<class std::basic_string<char,struct std::char_traits<char>,class std::allocator<char> >,class std::allocator<class std::basic_string<char,struct std::char_traits<char>,class std::allocator<char> > > > const &,class std::basic_string<char,struct std::char_traits<char>,class std::allocator<char> > *,int)" (?validate@program_options@boost@@YAXAAVany@2@ABV?$vector@V?$basic_string@DU?$char_traits@D@std@@V?$allocator@D@2@@std@@V?$allocator@V?$basic_string@DU?$char_traits@D@std@@V?$allocator@D@2@@std@@@2@@std@@PAV?$basic_string@DU?$char_traits@D@std@@V?$allocator@D@2@@5@H@Z) 已经在 boost_program_options-vc141-mt-gd-x32-1_66.lib(value_semantic.obj) 中定义
1>boost_program_options-vc140-mt-gd.lib(boost_program_options-vc141-mt-gd-x32-1_68.dll) : error LNK2005: "class std::basic_string<char,struct std::char_traits<char>,class std::allocator<char> > __cdecl boost::program_options::to_internal(class std::basic_string<char,struct std::char_traits<char>,class std::allocator<char> > const &)" (?to_internal@program_options@boost@@YA?AV?$basic_string@DU?$char_traits@D@std@@V?$allocator@D@2@@std@@ABV34@@Z) 已经在 boost_program_options-vc141-mt-gd-x32-1_66.lib(convert.obj) 中定义
1>  正在创建库 D:/code/VS/BabyCreator/eos_build/examples/Debug/fit-model-simple.lib 和对象 D:/code/VS/BabyCreator/eos_build/examples/Debug/fit-model-simple.exp
1>fit-model-simple.obj : error LNK2019: 无法解析的外部符号 "class cv::Mat __cdecl cv::imread(class std::basic_string<char,struct std::char_traits<char>,class std::allocator<char> > const &,int)" (?imread@cv@@YA?AVMat@1@ABV?$basic_string@DU?$char_traits@D@std@@V?$allocator@D@2@@std@@H@Z)，该符号在函数 __catch$_main$0 中被引用
1>fit-model-simple.obj : error LNK2019: 无法解析的外部符号 "bool __cdecl cv::imwrite(class std::basic_string<char,struct std::char_traits<char>,class std::allocator<char> > const &,class cv::debug_build_guard::_InputArray const &,class std::vector<int,class std::allocator<int> > const &)" (?imwrite@cv@@YA_NABV?$basic_string@DU?$char_traits@D@std@@V?$allocator@D@2@@std@@ABV_InputArray@debug_build_guard@1@ABV?$vector@HV?$allocator@H@std@@@3@@Z)，该符号在函数 __catch$_main$6 中被引用
1>D:\code\VS\BabyCreator\eos_build\examples\Debug\fit-model-simple.exe : fatal error LNK1120: 2 个无法解析的外部命令
```

### 原因

这个问题可能是安装的这个版本的 boost 库与什么东西不兼容吧，又是环境问题，我已经无力吐槽 Windows 了（心累）。

### 解决

这个问题我在官方仓库提了个 [issue](https://github.com/patrikhuber/eos/issues/245)。

作者的意思是：我没试过你的 boost 和 opencv 的版本，可能是本地配置问题，建议用构建系统（如 [vcpkg](https://github.com/Microsoft/vcpkg/)） 或者在 StackOverflow 上提问。

我：emmm......

所以，这个问题未解决，最后是用 vcpkg 安装库解决的。


## 再次编译环境

* Windows 10
* Visual Studio 2017
* CMake 3.12.4
* OpenCV 3.4.3
* Boost 1.68.0
* vcpkg 2018.11.23-nohash

## vcpkg 配置安装

安装 vcpkg、opencv、boost
```
git clone https://github.com/Microsoft/vcpkg.git
cd vcpkg
.\bootstrap-vcpkg.bat
.\vcpkg install opencv boost
```

安装完成后，在 `eos` 同级目录下创建 build 文件夹 `eos_vcpkg_build`，如下目录结构：

```
- eos
  - CMakeLists.txt
  - ...
- eos_vcpkg_build
  - install
  - eos.sln
  - ...
```

然后执行下面的命令构建工程：

```
mkdir eos_vcpkg_build
cd eos_vcpkg_build
cmake ../eos -DOpenCV_DIR=D:\\code\\git\\vcpkg\\buildtrees\\opencv\\x86-windows-rel -DBOOST_ROOT=D:\\code\\git\\vcpkg\\installed\\x86-windows\\bin -DBOOST_LIBRARYDIR=D:\\code\\git\\vcpkg\\installed\\x86-windows\\bin -DBOOST_INCLUDEDIR=D:\\code\\git\\vcpkg\\installed\\x86-windows\\include -DCMAKE_INSTALL_PREFIX=install/
```

* `OpenCV_DIR` 设为 vcpkg 安装的 `opencv 路径`
* `BOOST_ROOT` 设为 vcpkg 安装的 `boost 路径`
* `BOOST_LIBRARYDIR` 设为 vcpkg 安装的 `boost 库文件路径`
* `BOOST_INCLUDEDIR` 设为 vcpkg 安装的 `boost 头文件路径`
* `CMAKE_INSTALL_PREFIX` 即为 `make install 的路径`

构建好后，用 VS 打开，右键 `解决方案 eos` - `生成解决方案`

然后选择 `INSTALL` 工程 - 右键 `设为启动项目` - 右键 `生成`

居然成功编译并安装好了（喜极而泣）！

## eos 运行示例

运行下 `eos_vcpkg_build\install\bin\fit-model.exe` 示例程序吧 ~

根据墨菲定律，必定没这么顺利，果然......

一连跳几个框，

```
由于找不到 opencv_core343.dll、opencv_imgcodecs343.dll、boost_filesystem-vc141-mt-x32-1_68.dll、jpeg62.dll、zlib1.dll 等，无法继续执行代码。重新安装重新可能会解决此问题。
```

![](https://huihut-img.oss-cn-shenzhen.aliyuncs.com/1546520602.jpg)

既然缺少动态库，那就去 vcpkg 安装路径下找，如果没有则用 vcpkg 安装，然后把这些 dll 放到 `fit-model.exe` 同级目录下。

![](https://huihut-img.oss-cn-shenzhen.aliyuncs.com/2019-01-03_213217.png)

然后就......

```
应用程序无法正常启动(0xc000007b)。请点击“确定”关闭应用程序。
```

![](https://huihut-img.oss-cn-shenzhen.aliyuncs.com/2019-01-03_212959.png)

我：emmm......mmp

应该是动态库的链接错误，关于这个问题我再次开了 [issue](https://github.com/patrikhuber/eos/issues/245#issuecomment-451143760)，但是作者不再回应了。

所以，在 Windows 上，只是编译成功库，但是示例都无法运行。

最终，在 Ubuntu 上，编译运行起来了。

心累。