---

title: 使用MFC的CDC类绘制二维坐标系及正余弦函数
subtitle: 本文使用MFC的CDC类绘制二维坐标系及正余弦函数，可以进行坐标变换、规模变换，可以设置绘制的函数。通过输入自变量的范围及步长，绘制出相应的函数图形。
date: 2017-10-13 00:55:09
author: huihut
tags:
	- 图形学
	- MFC
categories: 
	- CS
	
---


## 系列链接

* [使用MFC的CDC类绘制二维坐标系及正余弦函数](https://blog.huihut.com/2017/10/13/GraphicsExercise2D/) / [源码](https://github.com/huihut/GraphicsExercise2D)

* [使用MFC的CDC类绘制三维坐标系及球面函数](https://blog.huihut.com/2017/10/13/GraphicsExercise3D/) / [源码](https://github.com/huihut/GraphicsExercise3D)

## 概述

本文使用MFC的CDC类绘制二维坐标系及正余弦函数，可以进行坐标变换、规模变换，可以设置绘制的函数。通过输入自变量的范围及步长，绘制出相应的函数图形。

<!-- more -->

## 新建项目

`Visual Studio`- `新建项目` - `MFC应用程序` - 命名为`CGraphicsExercise2D` - `确定` - `下一步` - 应用程序类型选择`单个文档` - `完成`

## 绘制函数

Visual Studio为我们创建了很多无用的代码，而我们的绘制函数在在`CGraphicsExercise2DView.cpp`的

    void CGraphicsExercise2DView::OnDraw(CDC* /*pDC*/)
    {
      CGraphicsExercise2DDoc* pDoc = GetDocument();
      ASSERT_VALID(pDoc);
      if (!pDoc)
      return;

      // TODO: 在此处为本机数据添加绘制代码
    }

取消`pDC`的注释，变成

    void CGraphicsExercise2DView::OnDraw(CDC* pDC)

在

    // TODO: 在此处为本机数据添加绘制代码

下面编写你自己的程序，如画一条线：

    pDC->MoveTo(20, 30);    // 画笔移到从左上角往右20像素、往下30像素
    pDC->LineTo(100, 100);    // 画一条线到右100、下100的位置

运行下看下效果吧！

现在删掉上面两行那条线，开始正式编写二维坐标系了。

## 规模变换函数

上面的`MoveTo(20, 30)`中的20、30是在显示器上的像素点，如果绘制的坐标系是以像素为大小的话，那1、2这样小的单位在显示器上就难以看到，因此需要规模变换。通常是把小单位乘上放大规模（倍数）就可以了。

在`CGraphicsExercise2DView.h`

    public:
      void SetScale(int scale);
      float TransformScale(float num);

    private:
  	  int scale;

在`CGraphicsExercise2DView.cpp`


    // 设置规模
    void CGraphicsExercise2DView::SetScale(int scale)
    {
      this->scale = scale;
    }

    // 变换规模
    float CGraphicsExercise2DView::TransformScale(float num)
    {
      return num * scale;
    }

并在`CGraphicsExercise2DView()`函数添加

    // 设置规模比例
    SetScale(70);

## 变换坐标和规模

在`CGraphicsExercise2DView.h`

    public:
      float TransformCoordinateScaleX(float x);
      float TransformCoordinateScaleY(float y);

在`CGraphicsExercise2DView.cpp`

    // 变换x的坐标和规模
    float CGraphicsExercise2DView::TransformCoordinateScaleX(float x)
    {
	     return TransformScale(x + 2);
    }

    // 变换y的坐标和规模
    float CGraphicsExercise2DView::TransformCoordinateScaleY(float y)
    {
	     return TransformScale(y + 4);
    }

## 设置绘制的函数类型

在`CGraphicsExercise2DView.h`

类外面定义

    // 支持绘制的函数类型
    enum Function { Sin, Cos };

类里面定义

    public:
      void SetDrawFunction(Function fun);

    private:
      Function fun;

在`GraphicsExerciseView.cpp`

    #include <math.h>

    // 设置绘制的函数
    void CGraphicsExercise2DView::SetDrawFunction(Function fun)
    {
	    this->fun = fun;
    }

并在`CGraphicsExercise2DView()`函数添加

    // 设置绘制的函数
    SetDrawFunction(Sin);

## 函数范围和步长

设置正余弦函数的x取值范围如`[0, 2*π]`，设置x的取样步长如`0.01`。

在`CGraphicsExercise2DView.h`

    public:
      void SetPlotSin(float startX, float endX, float step);

    private:
      float startX, endX, step;

在`CGraphicsExercise2DView.cpp`

    // 设置范围和步长
    void CGraphicsExercise2DView::SetPlotSin(float startX, float endX, float step)
    {
	     this->startX = startX;
	     this->endX = endX;
	     this->step = step;
    }

并在`CGraphicsExercise2DView()`函数添加

    // 设置自变量x范围[startX, endX]、取样步长step
    SetPlotSin((float)0.0, (float)6.3, (float)0.01);


## 绘制坐标系

坐标系是距离左上角右下各2 \* 规模个像素开始绘制的（即y轴的顶点是（2 \* 放大规模, 2 \* 放大规模））

在`OnDraw()`函数的`// TODO: 在此处为本机数据添加绘制代码`下面添加如下代码

```
// -------------------- 绘制坐标系 -------------------------

float endPointX = 2 + endX + 2;

// 坐标y轴
pDC->MoveTo((int)TransformScale(2), (int)TransformScale(2));
pDC->LineTo((int)TransformScale(2), (int)TransformScale(6));

// 坐标x轴
pDC->MoveTo((int)TransformScale(2), (int)TransformScale(4));
pDC->LineTo((int)TransformScale(endPointX), (int)TransformScale(4));

// 坐标y轴的箭头
pDC->MoveTo((int)TransformScale((float)1.8), (int)TransformScale((float)2.2));
pDC->LineTo((int)TransformScale(2), (int)TransformScale(2));
pDC->LineTo((int)TransformScale((float)2.2), (int)TransformScale((float)2.2));

// 坐标x轴的箭头
pDC->MoveTo((int)TransformScale(endPointX - (float)0.2), (int)TransformScale((float)3.8));
pDC->LineTo((int)TransformScale(endPointX), (int)TransformScale(4));
pDC->LineTo((int)TransformScale(endPointX - (float)0.2), (int)TransformScale((float)4.2));

// -------------------- 绘制刻度线 -------------------------

// 绘制y轴刻度线
for (float scaleY = 3; scaleY <= 5; scaleY += 0.2)
{
  pDC->MoveTo((int)TransformScale(2), (int)TransformScale(scaleY));
  pDC->LineTo((int)TransformScale((float)2.1), (int)TransformScale(scaleY));
}

// 绘制x轴刻度线
for (float scaleX = 2.2; scaleX < endPointX - 1; scaleX += 0.2)
{
  pDC->MoveTo((int)TransformScale(scaleX), (int)TransformScale(4));
  pDC->LineTo((int)TransformScale(scaleX), (int)TransformScale(3.9));
}


// -------------------- 绘制文字 -------------------------

// 绘制y轴的y
pDC->TextOutW((int)TransformScale(1.8), (int)TransformScale(2.3), CString("y"));
// 绘制x轴的x
pDC->TextOutW((int)TransformScale(endPointX - (float)0.5), (int)TransformScale(4.1), CString("x"));

CString s;
// 绘制y轴刻度文字
for (float ScaleTextY = 2.9, text = 1.0; ScaleTextY <= 4.9; ScaleTextY += 0.2, text -= 0.2)
{
  s.Format(_T("%.1f"), text);
  pDC->TextOutW((int)TransformScale(1.6), (int)TransformScale(ScaleTextY), s);
}

// 绘制x轴刻度文字
for (float ScaleTextX = 2.3; ScaleTextX < endPointX - 1; ScaleTextX += 0.4)
{
  s.Format(_T("%.1f"), ScaleTextX - 1.9);
  pDC->TextOutW((int)TransformScale(ScaleTextX), (int)TransformScale(4.1), s);
}

// 绘制函数图的Title
// 判断调用的函数
switch (fun)
{
case Sin:
  pDC->TextOutW((int)TransformScale(4), (int)TransformScale(6), CString("y = sin( x )"));
  break;
case Cos:
  pDC->TextOutW((int)TransformScale(4), (int)TransformScale(6), CString("y = cos( x )"));
  break;
default:
  break;
}
```

## 绘制函数

x从startX绘制到endX，每间隔step绘制一次。

也是在`OnDraw()`函数下面添加

```
// -------------------- 绘制函数 -------------------------

// 不改变坐标和规模的xy
float x, y;

for (x = startX; x <= endX; x += step)
{
  // 判断调用的函数
  switch (fun)
  {
  case Sin:
    y = (float)sin(x);
    break;
  case Cos:
    y = (float)cos(x);
    break;
  default:
    break;
  }

  // 对xy改变坐标和规模再显示点
  pDC->SetPixel((int)TransformCoordinateScaleX(x), (int)TransformCoordinateScaleY(y), 0);
}
```

## 效果图

![GraphicsExercise2DCapture](http://huihut-img.oss-cn-shenzhen.aliyuncs.com/GraphicsExercise2DCapture.png)
