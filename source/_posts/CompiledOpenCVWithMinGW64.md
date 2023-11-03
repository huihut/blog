---

title: OpenCV使用CMake和MinGW-w64的编译安装
subtitle: OpenCV使用CMake和MinGW-w64的编译安装
date: 2018-07-31 22:19:45
author: huihut
tags:
	- 链接装载库
categories: 
	- CS
	
---

## 前言

之前写过的一篇博文：[OpenCV使用CMake和MinGW的编译安装及其在Qt配置运行](https://blog.huihut.com/2017/12/03/CompiledOpenCVRunInQt/) 是使用 32 位的 MinGW 在 Windows 下编译 OpenCV 生成 32 位的 dll。

而这篇博文是使用 64 位的 MinGW 编译 OpenCV 生成 64 位的 dll。

因为博主没有 64 位 qmake，所以没勾选 `WITH_QT`

## 编译好的 OpenCV（MinGW 版）：

[Github . huihut/OpenCV-MinGW-Build](https://github.com/huihut/OpenCV-MinGW-Build)

<!-- more -->

## 软件环境

* Windows-10-64bit
* [MinGW-x64-4.8.1-release-posix-seh-rev5](http://sourceforge.net/projects/mingwbuilds/files/host-windows/releases/4.8.1/64-bit/threads-posix/seh/x64-4.8.1-release-posix-seh-rev5.7z/download)
* [CMake-3.12.0](https://cmake.org/download/)
* [OpenCV-3.4.1](https://opencv.org/releases.html) | [OpenCV-4.0.0-alpha](https://opencv.org/releases.html) | [OpenCV-4.0.0-rc](https://opencv.org/releases.html) | [OpenCV-4.1.0](https://opencv.org/releases.html)

## 安装 MinGW-w64 并配置环境变量

下载安装：[MinGW-x64-4.8.1-release-posix-seh-rev5](http://sourceforge.net/projects/mingwbuilds/files/host-windows/releases/4.8.1/64-bit/threads-posix/seh/x64-4.8.1-release-posix-seh-rev5.7z/download)

（博文使用 MinGW-x64-4.8.1 为例，但建议使用最新 MinGW：[MinGW-w64 - for 32 and 64 bit Windows](https://sourceforge.net/projects/mingw-w64/files/mingw-w64/mingw-w64-release/)）

为用户变量 `Path` 添加 `E:\MinGW-w64\x64-4.8.1-release-posix-seh-rev5\mingw64\bin`

## 使环境变量生效

打开命令提示符 CMD，运行 `set PATH=C:`，更改当前窗口任务的环境变量，关闭这个 CMD。

再次打开另一个 CMD，运行 `echo %PATH%`，显示最新的环境变量，会发现刚刚添加的 MinGW 环境变量已经生效。

## 使用 CMake 生成 OpenCV 的 Makefile

打开 cmake-gui，设置源码和生成路径：

* Where is the source code: `E:/opencv_341/opencv/sources`
* Where to build the binaries: `E:/opencv_341/opencv_mingw64_build`

点击 Configure，设置编译器

* Specify the generator for this project: `MinGW Makefiles`
* Specify native compilers
* Next
* Compilers C: `E:\MinGW-w64\x64-4.8.1-release-posix-seh-rev5\mingw64\bin\gcc.exe`
* Compilers C++: `E:\MinGW-w64\x64-4.8.1-release-posix-seh-rev5\mingw64\bin\g++.exe`
* Finish

编译配置：

* 勾选 `WITH_OPENGL`
* 勾选 `ENABLE_CXX11`
* 不勾选 `WITH_IPP`
* 不勾选 `ENABLE_PRECOMPILED_HEADERS`

点击 Configure，Generate 生成 Makefile

博主的配置信息如下：

```
General configuration for OpenCV 3.4.1 =====================================
  Version control:               unknown

  Platform:
    Timestamp:                   2018-07-31T02:14:11Z
    Host:                        Windows 10.0.17134 AMD64
    CMake:                       3.12.0
    CMake generator:             MinGW Makefiles
    CMake build tool:            E:/MinGW-w64/x64-4.8.1-release-posix-seh-rev5/mingw64/bin/mingw32-make.exe
    Configuration:               Release

  CPU/HW features:
    Baseline:                    SSE SSE2 SSE3
      requested:                 SSE3
    Dispatched code generation:  SSE4_1 SSE4_2 FP16 AVX AVX2
      requested:                 SSE4_1 SSE4_2 AVX FP16 AVX2 AVX512_SKX
      SSE4_1 (3 files):          + SSSE3 SSE4_1
      SSE4_2 (1 files):          + SSSE3 SSE4_1 POPCNT SSE4_2
      FP16 (2 files):            + SSSE3 SSE4_1 POPCNT SSE4_2 FP16 AVX
      AVX (5 files):             + SSSE3 SSE4_1 POPCNT SSE4_2 AVX
      AVX2 (9 files):            + SSSE3 SSE4_1 POPCNT SSE4_2 FP16 FMA3 AVX AVX2

  C/C++:
    Built as dynamic libs?:      YES
    C++11:                       YES
    C++ Compiler:                E:/MinGW-w64/x64-4.8.1-release-posix-seh-rev5/mingw64/bin/g++.exe  (ver 4.8.1)
    C++ flags (Release):         -fsigned-char -W -Wall -Werror=return-type -Werror=non-virtual-dtor -Werror=address -Werror=sequence-point -Wformat -Werror=format-security -Wmissing-declarations -Wundef -Winit-self -Wpointer-arith -Wshadow -Wsign-promo -Wuninitialized -Winit-self -Wno-narrowing -Wno-delete-non-virtual-dtor -Wno-comment -fdiagnostics-show-option -Wno-long-long -fomit-frame-pointer -ffunction-sections -fdata-sections  -msse -msse2 -msse3 -fvisibility=hidden -fvisibility-inlines-hidden -O3 -DNDEBUG  -DNDEBUG
    C++ flags (Debug):           -fsigned-char -W -Wall -Werror=return-type -Werror=non-virtual-dtor -Werror=address -Werror=sequence-point -Wformat -Werror=format-security -Wmissing-declarations -Wundef -Winit-self -Wpointer-arith -Wshadow -Wsign-promo -Wuninitialized -Winit-self -Wno-narrowing -Wno-delete-non-virtual-dtor -Wno-comment -fdiagnostics-show-option -Wno-long-long -fomit-frame-pointer -ffunction-sections -fdata-sections  -msse -msse2 -msse3 -fvisibility=hidden -fvisibility-inlines-hidden -g  -O0 -DDEBUG -D_DEBUG
    C Compiler:                  E:/MinGW-w64/x64-4.8.1-release-posix-seh-rev5/mingw64/bin/gcc.exe
    C flags (Release):           -fsigned-char -W -Wall -Werror=return-type -Werror=non-virtual-dtor -Werror=address -Werror=sequence-point -Wformat -Werror=format-security -Wmissing-declarations -Wmissing-prototypes -Wstrict-prototypes -Wundef -Winit-self -Wpointer-arith -Wshadow -Wuninitialized -Winit-self -Wno-narrowing -Wno-comment -fdiagnostics-show-option -Wno-long-long -fomit-frame-pointer -ffunction-sections -fdata-sections  -msse -msse2 -msse3 -fvisibility=hidden -O3 -DNDEBUG  -DNDEBUG
    C flags (Debug):             -fsigned-char -W -Wall -Werror=return-type -Werror=non-virtual-dtor -Werror=address -Werror=sequence-point -Wformat -Werror=format-security -Wmissing-declarations -Wmissing-prototypes -Wstrict-prototypes -Wundef -Winit-self -Wpointer-arith -Wshadow -Wuninitialized -Winit-self -Wno-narrowing -Wno-comment -fdiagnostics-show-option -Wno-long-long -fomit-frame-pointer -ffunction-sections -fdata-sections  -msse -msse2 -msse3 -fvisibility=hidden -g  -O0 -DDEBUG -D_DEBUG
    Linker flags (Release):      -Wl,--gc-sections  
    Linker flags (Debug):        -Wl,--gc-sections  
    ccache:                      NO
    Precompiled headers:         NO
    Extra dependencies:          opengl32 glu32
    3rdparty dependencies:

  OpenCV modules:
    To be built:                 calib3d core dnn features2d flann highgui imgcodecs imgproc java_bindings_generator ml objdetect photo python_bindings_generator shape stitching superres ts video videoio videostab
    Disabled:                    js world
    Disabled by dependency:      -
    Unavailable:                 cudaarithm cudabgsegm cudacodec cudafeatures2d cudafilters cudaimgproc cudalegacy cudaobjdetect cudaoptflow cudastereo cudawarping cudev java python2 python3 viz
    Applications:                tests perf_tests apps
    Documentation:               NO
    Non-free algorithms:         NO

  Windows RT support:            NO

  GUI: 
    Win32 UI:                    YES
    OpenGL support:              YES (opengl32 glu32)
    VTK support:                 NO

  Media I/O: 
    ZLib:                        build (ver 1.2.11)
    JPEG:                        build (ver 90)
    WEBP:                        build (ver encoder: 0x020e)
    PNG:                         build (ver 1.6.34)
    TIFF:                        build (ver 42 - 4.0.9)
    JPEG 2000:                   build (ver 1.900.1)
    OpenEXR:                     build (ver 1.7.1)

  Video I/O:
    Video for Windows:           YES
    DC1394:                      NO
    FFMPEG:                      YES (prebuilt binaries)
      avcodec:                   YES (ver 57.107.100)
      avformat:                  YES (ver 57.83.100)
      avutil:                    YES (ver 55.78.100)
      swscale:                   YES (ver 4.8.100)
      avresample:                YES (ver 3.7.0)
    GStreamer:                   NO
    DirectShow:                  YES

  Parallel framework:            none

  Trace:                         YES (built-in)

  Other third-party libraries:
    Lapack:                      NO
    Eigen:                       NO
    Custom HAL:                  NO
    Protobuf:                    build (3.5.1)

  NVIDIA CUDA:                   NO

  OpenCL:                        YES (no extra features)
    Include path:                E:/opencv_341/opencv/sources/3rdparty/include/opencl/1.2
    Link libraries:              Dynamic load

  Python (for build):            E:/Python37-32/python.exe

  Java:                          
    ant:                         NO
    JNI:                         C:/Program Files (x86)/Java/jdk1.8.0_181/include C:/Program Files (x86)/Java/jdk1.8.0_181/include/win32 C:/Program Files (x86)/Java/jdk1.8.0_181/include
    Java wrappers:               NO
    Java tests:                  NO

  Matlab:                        NO

  Install to:                    E:/opencv_341/opencv_mingw64_build/install
-----------------------------------------------------------------

Configuring done
Generating done
```

## 编译 OpenCV

打开终端进行编译：（`-j` 是使用 `8` 个线程进行编译，请根据你的计算机配置合理设置线程数）

```
E:
cd E:\opencv_341\opencv_mingw64_build
mingw32-make -j 8
mingw32-make install
```

如果 `mingw32-make -j 8` 遇到错误，请看下面的  **编译 OpenCV 常见错误**，否则执行 `mingw32-make install`，完成安装。

## 编译 OpenCV 常见错误

### 1. MinGW-w64 的 aviriff.h 文件注释错误

#### 表现

```
[ 49%] Building CXX object modules/videoio/CMakeFiles/opencv_videoio.dir/src/cap_dshow.cpp.obj
In file included from E:\opencv_341\opencv\sources\modules\videoio\src\cap_dshow.cpp:113:0:
e:\mingw-w64\x64-4.8.1-release-posix-seh-rev5\mingw64\x86_64-w64-mingw32\include\aviriff.h:2:8: error: expected constructor, destructor, or type conversion before 'file'
 * This file is part of the mingw-w64 runtime package.
        ^
e:\mingw-w64\x64-4.8.1-release-posix-seh-rev5\mingw64\x86_64-w64-mingw32\include\aviriff.h:3:25: error: 'refer' does not name a type
 * No warranty is given; refer to the file DISCLAIMER within this package.
                         ^
In file included from e:\mingw-w64\x64-4.8.1-release-posix-seh-rev5\mingw64\x86_64-w64-mingw32\include\aviriff.h:19:0,
                 from E:\opencv_341\opencv\sources\modules\videoio\src\cap_dshow.cpp:113:
e:\mingw-w64\x64-4.8.1-release-posix-seh-rev5\mingw64\x86_64-w64-mingw32\include\pshpack2.h:7:21: error: expected declaration before end of line
 #pragma pack(push,2)
                     ^
modules\videoio\CMakeFiles\opencv_videoio.dir\build.make:146: recipe for target 'modules/videoio/CMakeFiles/opencv_videoio.dir/src/cap_dshow.cpp.obj' failed
mingw32-make[2]: *** [modules/videoio/CMakeFiles/opencv_videoio.dir/src/cap_dshow.cpp.obj] Error 1
CMakeFiles\Makefile2:3057: recipe for target 'modules/videoio/CMakeFiles/opencv_videoio.dir/all' failed
mingw32-make[1]: *** [modules/videoio/CMakeFiles/opencv_videoio.dir/all] Error 2
Makefile:161: recipe for target 'all' failed
mingw32-make: *** [all] Error 2
```

![MinGW-w64_aviriff.h_file_annotation_error](http://huihut-img.oss-cn-shenzhen.aliyuncs.com/MinGW-w64_aviriff.h_file_annotation_error.png)

#### 解决

打开`E:\MinGW-w64\x64-4.8.1-release-posix-seh-rev5\mingw64\x86_64-w64-mingw32\include\aviriff.h`

发现第一行的多行注释少了个`/`符号，加上保存，如下：

```cpp
/**
* This file is part of the mingw-w64 runtime package.
* No warranty is given; refer to the file DISCLAIMER within this package.
*/
```

然后重新 `Configure`-`Generate`-`mingw32-make` 就好了。

### 2. cap_msmf.cpp capture code 错误【2018年10月13日修改，因编译 OpenCV-4.0.0-alpha 时遇到并解决】

#### 表现

```
......
E:\opencv-4.0.0-alpha\opencv-4.0.0-alpha\modules\videoio\src\cap_msmf.cpp: In member function 'HRESULT {anonymous}::ComPtr<T>::As({anonymous}::ComPtr<U>&) const [with U = IMF2DBuffer; T = IMFMediaBuffer; HRESULT = long int]':
E:\opencv-4.0.0-alpha\opencv-4.0.0-alpha\modules\videoio\src\cap_msmf.cpp:172:5: error: control reaches end of non-void function [-Werror=return-type]
     }
     ^
E:\opencv-4.0.0-alpha\opencv-4.0.0-alpha\modules\videoio\src\cap_msmf.cpp: In member function 'T* {anonymous}::ComPtr<T>::operator->() const [with T = IMF2DBuffer]':
E:\opencv-4.0.0-alpha\opencv-4.0.0-alpha\modules\videoio\src\cap_msmf.cpp:149:5: error: control reaches end of non-void function [-Werror=return-type]
     }
     ^
E:\opencv-4.0.0-alpha\opencv-4.0.0-alpha\modules\videoio\src\cap_msmf.cpp: In member function 'T* {anonymous}::ComPtr<T>::operator->() const [with T = IMFMediaBuffer]':
E:\opencv-4.0.0-alpha\opencv-4.0.0-alpha\modules\videoio\src\cap_msmf.cpp:149:5: error: control reaches end of non-void function [-Werror=return-type]
     }
     ^
E:\opencv-4.0.0-alpha\opencv-4.0.0-alpha\modules\videoio\src\cap_msmf.cpp: In member function 'T* {anonymous}::ComPtr<T>::operator->() const [with T = IMFSinkWriter]':
E:\opencv-4.0.0-alpha\opencv-4.0.0-alpha\modules\videoio\src\cap_msmf.cpp:149:5: error: control reaches end of non-void function [-Werror=return-type]
     }
     ^
E:\opencv-4.0.0-alpha\opencv-4.0.0-alpha\modules\videoio\src\cap_msmf.cpp: In member function 'T* {anonymous}::ComPtr<T>::Get() const [with T = IMFMediaBuffer]':
E:\opencv-4.0.0-alpha\opencv-4.0.0-alpha\modules\videoio\src\cap_msmf.cpp:158:5: error: control reaches end of non-void function [-Werror=return-type]
     }
     ^
cc1plus.exe: some warnings being treated as errors
modules\videoio\CMakeFiles\opencv_videoio.dir\build.make:188: recipe for target 'modules/videoio/CMakeFiles/opencv_videoio.dir/src/cap_msmf.cpp.obj' failed
mingw32-make[2]: *** [modules/videoio/CMakeFiles/opencv_videoio.dir/src/cap_msmf.cpp.obj] Error 1
CMakeFiles\Makefile2:2556: recipe for target 'modules/videoio/CMakeFiles/opencv_videoio.dir/all' failed
mingw32-make[1]: *** [modules/videoio/CMakeFiles/opencv_videoio.dir/all] Error 2
mingw32-make[1]: *** Waiting for unfinished jobs....
[ 58%] Linking CXX shared library ..\..\bin\libopencv_dnn400.dll
[ 59%] Built target opencv_dnn
Makefile:161: recipe for target 'all' failed
mingw32-make: *** [all] Error 2
```

![](http://huihut-img.oss-cn-shenzhen.aliyuncs.com/20180925220634.png)

#### 原因

因为 MinGW 不能编译 OpenCV 的 cap_msmf 那部分代码，具体见我提的 Issue：[Failed to compile opencv-4.0.0-alpha using mingw-w64](https://github.com/opencv/opencv/issues/12642)。

#### 解决

所以，在 cmake-gui 编译配置中：

* 不勾选 `WITH_MSMF`

然后重新 `Configure`-`Generate`-`mingw32-make`

### 3. 'M_PI' was not declared in this scope 错误【2018年10月13日修改，因编译 OpenCV-4.0.0-alpha 时遇到并解决】

#### 表现

```
[ 86%] Building CXX object modules/calib3d/CMakeFiles/opencv_calib3d.dir/src/chessboard.cpp.obj
In file included from E:\opencv-4.0.0-alpha\opencv-4.0.0-alpha\modules\calib3d\src\chessboard.cpp:7:0:
E:\opencv-4.0.0-alpha\opencv-4.0.0-alpha\modules\calib3d\src\chessboard.hpp: In constructor 'cv::details::FastX::Parameters::Parameters()':
E:\opencv-4.0.0-alpha\opencv-4.0.0-alpha\modules\calib3d\src\chessboard.hpp:35:36: error: 'M_PI' was not declared in this scope
                 resolution = float(M_PI*0.25);
                                    ^
E:\opencv-4.0.0-alpha\opencv-4.0.0-alpha\modules\calib3d\src\chessboard.cpp: At global scope:
E:\opencv-4.0.0-alpha\opencv-4.0.0-alpha\modules\calib3d\src\chessboard.cpp:25:42: error: 'M_PI' was not declared in this scope
 const float MAX_ANGLE = float(48.0/180.0*M_PI);          // max angle between line segments supposed to be straight
                                          ^
E:\opencv-4.0.0-alpha\opencv-4.0.0-alpha\modules\calib3d\src\chessboard.cpp:26:48: error: 'M_PI' was not declared in this scope
 const float MIN_COS_ANGLE = float(cos(35.0/180*M_PI));   // min cos angle between board edges
                                                ^
E:\opencv-4.0.0-alpha\opencv-4.0.0-alpha\modules\calib3d\src\chessboard.cpp:29:35: error: 'M_PI' was not declared in this scope
 const float RAD2DEG = float(180.0/M_PI);
                                   ^
E:\opencv-4.0.0-alpha\opencv-4.0.0-alpha\modules\calib3d\src\chessboard.cpp: In function 'int cv::details::testPointSymmetry(cv::Mat, cv::Point2f, float, float)':
E:\opencv-4.0.0-alpha\opencv-4.0.0-alpha\modules\calib3d\src\chessboard.cpp:217:33: error: 'M_PI' was not declared in this scope
     for(double angle=0;angle <= M_PI;angle+=M_PI*0.1)
                                 ^
E:\opencv-4.0.0-alpha\opencv-4.0.0-alpha\modules\calib3d\src\chessboard.cpp: In member function 'void cv::details::FastX::rotate(float, const cv::Mat&, cv::Size, cv::Mat&) const':
E:\opencv-4.0.0-alpha\opencv-4.0.0-alpha\modules\calib3d\src\chessboard.cpp:287:110: error: 'M_PI' was not declared in this scope
         cv::Mat m = cv::getRotationMatrix2D(cv::Point2f(float(img.cols*0.5),float(img.rows*0.5)),float(angle/M_PI*180),1);
                                                                                                              ^
E:\opencv-4.0.0-alpha\opencv-4.0.0-alpha\modules\calib3d\src\chessboard.cpp: In member function 'std::vector<std::vector<float> > cv::details::FastX::calcAngles(const std::vector<cv::Mat>&, std::vector<cv::KeyPoint>&) const':
E:\opencv-4.0.0-alpha\opencv-4.0.0-alpha\modules\calib3d\src\chessboard.cpp:393:30: error: 'M_PI' was not declared in this scope
     float resolution = float(M_PI/channels);
                              ^
E:\opencv-4.0.0-alpha\opencv-4.0.0-alpha\modules\calib3d\src\chessboard.cpp: In member function 'void cv::details::FastX::detectImpl(const cv::Mat&, std::vector<cv::Mat>&, std::vector<cv::Mat>&, const cv::Mat&) const':
E:\opencv-4.0.0-alpha\opencv-4.0.0-alpha\modules\calib3d\src\chessboard.cpp:631:30: error: 'M_PI' was not declared in this scope
         int num = int(0.5001*M_PI/parameters.resolution);
                              ^
E:\opencv-4.0.0-alpha\opencv-4.0.0-alpha\modules\calib3d\src\chessboard.cpp: In member function 'void cv::details::Ellipse::draw(cv::InputOutputArray, const Scalar&) const':
E:\opencv-4.0.0-alpha\opencv-4.0.0-alpha\modules\calib3d\src\chessboard.cpp:721:43: error: 'M_PI' was not declared in this scope
     cv::ellipse(img,center,axes,360-angle/M_PI*180,0,360,color);
                                           ^
E:\opencv-4.0.0-alpha\opencv-4.0.0-alpha\modules\calib3d\src\chessboard.cpp: In static member function 'static float cv::details::Chessboard::Board::findMaxPoint(cv::flann::Index&, const cv::Mat&, const cv::details::Ellipse&, float, float, cv::Point2f&)':
E:\opencv-4.0.0-alpha\opencv-4.0.0-alpha\modules\calib3d\src\chessboard.cpp:1541:17: error: 'M_PI' was not declared in this scope
         if(a1 > M_PI*0.5)
                 ^
E:\opencv-4.0.0-alpha\opencv-4.0.0-alpha\modules\calib3d\src\chessboard.cpp:1543:16: error: 'M_PI' was not declared in this scope
         if(a2> M_PI*0.5)
                ^
E:\opencv-4.0.0-alpha\opencv-4.0.0-alpha\modules\calib3d\src\chessboard.cpp: In static member function 'static bool cv::details::Chessboard::Board::estimateSearchArea(const Point2f&, const Point2f&, const Point2f&, float, cv::details::Ellipse&, const Point2f*)':
E:\opencv-4.0.0-alpha\opencv-4.0.0-alpha\modules\calib3d\src\chessboard.cpp:1787:28: error: 'M_PI' was not declared in this scope
         angle = float(2.0F*M_PI-angle);
                            ^
E:\opencv-4.0.0-alpha\opencv-4.0.0-alpha\modules\calib3d\src\chessboard.cpp: In member function 'void cv::details::Chessboard::findKeyPoints(const cv::Mat&, std::vector<cv::KeyPoint>&, std::vector<cv::Mat>&, std::vector<std::vector<float> >&, const cv::Mat&) const':
E:\opencv-4.0.0-alpha\opencv-4.0.0-alpha\modules\calib3d\src\chessboard.cpp:2793:29: error: 'M_PI' was not declared in this scope
     para.resolution = float(M_PI*0.25);   // this gives the best results taking interpolation into account
                             ^
E:\opencv-4.0.0-alpha\opencv-4.0.0-alpha\modules\calib3d\src\chessboard.cpp: In member function 'cv::Mat cv::details::Chessboard::buildData(const std::vector<cv::KeyPoint>&) const':
E:\opencv-4.0.0-alpha\opencv-4.0.0-alpha\modules\calib3d\src\chessboard.cpp:2844:30: error: 'M_PI' was not declared in this scope
         (*val++) = float(2.0*M_PI-iter->angle/180.0*M_PI);
                              ^
E:\opencv-4.0.0-alpha\opencv-4.0.0-alpha\modules\calib3d\src\chessboard.cpp: In member function 'std::vector<cv::KeyPoint> cv::details::Chessboard::getInitialPoints(cv::flann::Index&, const cv::Mat&, const cv::KeyPoint&, float, float, float) const':
E:\opencv-4.0.0-alpha\opencv-4.0.0-alpha\modules\calib3d\src\chessboard.cpp:2874:25: error: 'M_PI' was not declared in this scope
         if(angle_temp > M_PI*0.5)
                         ^
E:\opencv-4.0.0-alpha\opencv-4.0.0-alpha\modules\calib3d\src\chessboard.cpp:2879:29: error: 'M_PI' was not declared in this scope
             if(angle_temp > M_PI*0.5)
                             ^
modules\calib3d\CMakeFiles\opencv_calib3d.dir\build.make:137: recipe for target 'modules/calib3d/CMakeFiles/opencv_calib3d.dir/src/chessboard.cpp.obj' failed
mingw32-make[2]: *** [modules/calib3d/CMakeFiles/opencv_calib3d.dir/src/chessboard.cpp.obj] Error 1
CMakeFiles\Makefile2:3018: recipe for target 'modules/calib3d/CMakeFiles/opencv_calib3d.dir/all' failed
mingw32-make[1]: *** [modules/calib3d/CMakeFiles/opencv_calib3d.dir/all] Error 2
Makefile:161: recipe for target 'all' failed
mingw32-make: *** [all] Error 2
```

![](http://huihut-img.oss-cn-shenzhen.aliyuncs.com/20180925221908.png)

#### 原因

因为 OpenCV 在 `chessboard.cpp`、`chessboard.hpp`、`test_chesscorners.cpp` 这些代码中有 BUG，使用的应该是 `CV_PI` 而不是 `M_PI` 导致的。具体见我提的 Issue：[Failed to compile opencv-4.0.0-alpha using mingw-w64](https://github.com/opencv/opencv/issues/12642)

#### 解决

* 在最新的 master 分支已经解决了这个问题，见我的 PR ：[M_PI changed to CV_PI](https://github.com/opencv/opencv/pull/12645)

* 如果你是在 [官网](https://opencv.org/releases.html) 或者 [github.com/opencv/opencv/releases](https://github.com/opencv/opencv/releases) 中下的 `OpenCV 4.0.0-alpha`，可能还会有这个问题，那么你需要把 `chessboard.cpp`、`chessboard.hpp`、`test_chesscorners.cpp` 文件中的 `M_PI` 全部改为 `CV_PI`，如我的 commit 所示：[M_PI changed to CV_PI (#12645)](https://github.com/opencv/opencv/commit/f0d277e45246762866daea036558e2c391b39ace)

然后重新 `Configure`-`Generate`-`mingw32-make`

### 4. 'posix_memalign' was not declared in this scope 错误【2018年11月17日修改，因编译 OpenCV-4.0.0-rc 时遇到并解决】

#### 表现

```
[ 28%] Building CXX object modules/CMakeFiles/ade.dir/__/3rdparty/ade/ade-0.1.1c/sources/ade/source/alloc.cpp.obj
E:\opencv-4.0.0-rc\opencv-4.0.0-rc-build\3rdparty\ade\ade-0.1.1c\sources\ade\source\alloc.cpp: In function 'void* ade::aligned_alloc(std::size_t, std::size_t)':
E:\opencv-4.0.0-rc\opencv-4.0.0-rc-build\3rdparty\ade\ade-0.1.1c\sources\ade\source\alloc.cpp:31:16: error: 'posix_memalign' was not declared in this scope
     auto res = posix_memalign(&ret, std::max(sizeof(void*), alignment), size);
                ^~~~~~~~~~~~~~
mingw32-make[2]: *** [modules\CMakeFiles\ade.dir\build.make:63: modules/CMakeFiles/ade.dir/__/3rdparty/ade/ade-0.1.1c/sources/ade/source/alloc.cpp.obj] Error 1
mingw32-make[1]: *** [CMakeFiles\Makefile2:884: modules/CMakeFiles/ade.dir/all] Error 2
mingw32-make: *** [Makefile:162: all] Error 2
```

![2018-11-17_174118.png](https://huihut-img.oss-cn-shenzhen.aliyuncs.com/2018-11-17_174118.png)

#### 原因

因为我使用了 `MinGW-w64-8.1.0` 编译，而新的编译器在 Windows 下不再定义 `WIN32`，而定义成 `_WIN32`，如这个 Issue 的问题：[error: 'posix_memalign' was not declared in this scope #12831](https://github.com/opencv/opencv/issues/12831)

#### 解决

把 `opencv-4.0.0-rc-build\3rdparty\ade\ade-0.1.1c\sources\ade\source\alloc.cpp` 文件的所有 `WIN32` 改为 `_WIN32`，如这个 PR 所做的修改：[fix check for win32 #6](https://github.com/opencv/ade/pull/6/files)

然后重新 `Configure`-`Generate`-`mingw32-make`

### 5. 'D3D11_TEXTURE2D_DESC' was not declared in this scope 错误【2019年4月10日修改，因编译 OpenCV-4.1.0 时遇到并解决】

#### 表现

```
[ 32%] Building CXX object modules/core/CMakeFiles/opencv_core.dir/src/directx.cpp.obj
E:\opencv-4.1.0\opencv-4.1.0\modules\core\src\directx.cpp: In function 'void cv::directx::__convertToD3D11Texture2DNV(cv::InputAray, ID3D11Texture2D*)':
E:\opencv-4.1.0\opencv-4.1.0\modules\core\src\directx.cpp:1035:5: error: 'D3D11_TEXTURE2D_DESC' was not declared in this scope
     D3D11_TEXTURE2D_DESC desc = { 0 };
     ^~~~~~~~~~~~~~~~~~~~
```
![MinGW-w64_D3D11_TEXTURE2D_DESC_Error.png](https://huihut-img.oss-cn-shenzhen.aliyuncs.com/MinGW-w64_D3D11_TEXTURE2D_DESC_Error.png)

#### 原因

`D3D11_TEXTURE2D_DESC` 在 `d3d11.h` 中定义，貌似在我的机器上找不到定义。反正也不用 D3D11  与 OpenCL 交互，因此可以关闭 `WITH_OPENCL_D3D11_NV ` 这个选项（默认是开的）。可见我的 issue：[Error compiling 4.1.0 source code with mingw-w64: 'D3D11_TEXTURE2D_DESC' was not declared in this scope #14286](https://github.com/opencv/opencv/issues/14286)

#### 解决
方法一：

在 cmake-gui 编译配置中：

* 不勾选 `WITH_OPENCL_D3D11_NV `

然后重新 `Configure`-`Generate`-`mingw32-make`

方法二：

如这个 PR 中对这两个 cmake 文件的修改：[cmake: fix WITH_OPENCL_D3D11_NV check #14294](https://github.com/opencv/opencv/pull/14294/files)，即可先判断是否可以用，再设置默认的 `WITH_OPENCL_D3D11_NV`


### 其他错误

如果以上错误不是你所遇到的，请务必先看下面两个文章中的错误。

* [编译 32位 OpenCV 博文的常见错误](https://blog.huihut.com/2017/12/03/CompiledOpenCVRunInQt/)
* [Tutorial: Installation from source for Windows with Mingw-w64](http://visp-doc.inria.fr/doxygen/visp-daily/tutorial-install-win10-mingw64.html)

## 更新日志

1. 2018年10月13日为 OpenCV-4.0.0-alpha 而修改，主要改了 “编译 OpenCV 常见错误”
2. 2018年11月17日为 OpenCV-4.0.0-rc 而修改，主要改了 “编译 OpenCV 常见错误”
3. 2019年4月10日为 OpenCV-4.1.0 而修改，主要改了 “编译 OpenCV 常见错误”