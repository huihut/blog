---

title: CloudCompare插件编写三（算法实现）
subtitle: CloudComapre插件的编写，理解CloudComapre插件编写及点云数据结构，实现qSAF插件
date: 2017-04-27 10:35:43
author: huihut
tags:
	- QT
	- CloudCompare
categories: 
	- CS
	
---

## 唠叨

本文分三篇来介绍一个完整的CloudComapre插件的编写教程，分别是[插件框架篇](https://blog.huihut.com/2017/04/27/CloudCompareSAFPlugin_1_Framework/)、[数据结构篇](https://blog.huihut.com/2017/04/27/CloudCompareSAFPlugin_2_DataStructure/)、[算法实现篇](https://blog.huihut.com/2017/04/27/CloudCompareSAFPlugin_3_Algorithm/)。

这是第三篇，**算法实现篇**，你可以根据本文改成自己的插件，待卿临幸。

**特别注意**：本文的CloudCompare源码构建的是Qt工程并使用Qt Creator开发，并不是Visual Studio。

qSAF源码：[Github . qSAF](https://github.com/huihut/qSAF)

## 前文概要

在上回中，我们知道了点云中扫描角度的存储结构，下面我们来讲qSAF的具体实现。

## UI界面

新建QT设计器界面类，命名为`ccSAFDlg`，在`ccSAFDlg.ui`文件设计简单的界面。

因为我们只需要一个范围，一个确认取消键，所以我把它弄成这样子：

<!-- more -->

![](http://huihut-img.oss-cn-shenzhen.aliyuncs.com/SAFDlg.ui.jpg)

`doubleSpinBox`要设置范围：`0.00`到`90.00`，默认值分别设为`20.00`和`70.00`。

`ccSAFDlg.h`：

	#ifndef CCSAFDLG_H
	#define CCSAFDLG_H

	#include "ui_SAFDlg.h"
	#include <QDialog>

	namespace Ui {
	class ccSAFDlg;
	}

	class ccSAFDlg : public QDialog, public Ui::ccSAFDlg
	{
    	Q_OBJECT

	public:
    	explicit ccSAFDlg(QWidget *parent = 0);

	protected slots:

    	//! Saves (temporarily) the dialog paramters on acceptation
    	void saveSettings();
	};

	#endif // CCSAFDLG_H

`ccSAFDlg.cpp`：

	#include "ccSAFDlg.h"

	//定义两个静态阈值，并初始化
	static double threshold_1 = 20;
	static double threshold_2 = 70;

	ccSAFDlg::ccSAFDlg(QWidget *parent) : QDialog(parent), Ui::ccSAFDlg()
	{
    	setupUi(this);

		//关联信号槽
    	connect(buttonBox, SIGNAL(accepted()), this, SLOT(saveSettings()));

		//初始化设置阈值
    	doubleSpinBox_1->setValue(threshold_1);
    	doubleSpinBox_2->setValue(threshold_2);
	}

	void ccSAFDlg::saveSettings()
	{
		//OK后重新赋值
    	threshold_1 = doubleSpinBox_1->value();
    	threshold_2 = doubleSpinBox_2->value();
	}

现在界面就做好了。

## 插件doAction实现

至于doAction的实现，点云其中的数据结构，可以参考第二篇，[数据结构篇](https://blog.huihut.com/2017/04/27/CloudCompareSAFPlugin_2_DataStructure/)

简单地说，我们需要：

1. 用`Scan Angle Rank`，通过`getScalarFieldIndexByName()`获得扫描角度在标量域中的索引
2. 用索引，通过`getScalarField()`获得扫描角度标量域指针
3. 用指针，通过`getValue()`获得每个点的值
4. 比较扫描角度值与用户输入区间的大小，把合适的值存储起来
5. 把合适值封装成点云实体
6. 显示在界面上

大体的算法思路上是没有问题的，但是有个纠结的地方，就是是否使用进度条。

实测SAF处理一个雷达文件，

* 使用进度条耗时：129.1s
* 不用进度条耗时：3.5s

这种压倒性的差距让我果断砍掉真·进度条，没错！我使用假·进度条，就是不会动的进度条。

这样短时间的处理使用假·进度条，既不会降低处理速度，也不会降低用户体验~

下面就是完整代码，注释中有真·进度条的实现（`[进度条]`），但不推荐使用


	void qSAF::doAction()
	{
		//当插件加载时，m_app应该已经被CC初始化了
		assert(m_app);
		if (!m_app)
			return;

		//获取选择的实体
    	const ccHObject::Container& selectedEntities = m_app->getSelectedEntities();
    	//获取选择的实体数量
    	size_t selNum = selectedEntities.size();
    	//确保只选择一个实体
    	if (selNum != 1)
    	{
        	m_app->dispToConsole("[SAF] Select only one cloud!", ccMainAppInterface::ERR_CONSOLE_MESSAGE);
        	return;
    	}

    	ccHObject* ent = selectedEntities[0];
    	assert(ent);
    	//确保选择的实体是POINT_CLOUD类型
    	if (!ent || !ent->isA(CC_TYPES::POINT_CLOUD))
    	{
        	m_app->dispToConsole("[SAF] Select a real point cloud!", ccMainAppInterface::ERR_CONSOLE_MESSAGE);
        	return;
    	}

    	//从选择的实体中转换成ccPointCloud*类型
    	ccPointCloud* pc = static_cast<ccPointCloud*>(ent);

    	//获取点云的数量m_count
    	unsigned count = pc->size();

    	//初始化阈值变量
    	static double threshold_1 = 20;
    	static double threshold_2 = 70;
    	double threshold_temp = 0;

    	//显示插件ui窗体
    	{
        	ccSAFDlg safDlg(m_app->getMainWindow());
        	safDlg.doubleSpinBox_1->setValue(threshold_1);
        	safDlg.doubleSpinBox_2->setValue(threshold_2);

        	if(!safDlg.exec())
        	{
            	return;
        	}

        	//存储阈值
        	threshold_1 = safDlg.doubleSpinBox_1->value();
        	threshold_2 = safDlg.doubleSpinBox_2->value();
    	}

    	//显示进度条窗体
    	QProgressDialog pDlg;
    	pDlg.setWindowTitle("SAF");
    	pDlg.setLabelText(QString("Scan Angle Filter\nfrom %1 to %2").arg(threshold_1).arg(threshold_2));
    	//[进度条]设置进度条总范围
    	//pDlg.setRange(0, count);
    	pDlg.setCancelButton(0);
    	pDlg.show();
    	QApplication::processEvents();
		
    	QElapsedTimer timer;
    	//计时开始
    	timer.start();

    	ScalarType scanAngle;

    	CCLib::ReferenceCloud rangeAnglerc(pc);

    	//确保 threshold_1 小于 threshold_2
    	if(threshold_1 > threshold_2)
    	{
        	threshold_temp = threshold_1;
        	threshold_1 = threshold_2;
        	threshold_2 = threshold_temp;
    	}

    	//[进度条]进度条的取消SAF按钮
    	//bool wasCancelled = false;

    	//获取 Scan Angle Rank 的索引
    	int scanAngleSFIndex = pc->getScalarFieldIndexByName("Scan Angle Rank");

		//[重点]遍历每个点的操作
    	for(unsigned i = 0; i < count; ++i)
    	{
        	//获取每个点的扫描角度
        	scanAngle = pc->getScalarField(scanAngleSFIndex)->getValue(i);

        	//取扫描角度的绝对值
        	if(scanAngle < 0)
        	{
            	scanAngle = -scanAngle;
        	}

        	//如果扫描角度在给定的阈值范围，则添加它的索引到参考云
        	if(threshold_1 <= scanAngle && scanAngle <= threshold_2)
        	{
            	rangeAnglerc.addPointIndex(i);
        	}

	//        //[进度条]重置进度条
	//        pDlg.setValue(i);
	//        QCoreApplication::processEvents();

	//        //[进度条]取消SAF处理
	//        if (pDlg.wasCanceled())
	//        {
	//            wasCancelled = true;
	//            break;
	//        }
    	}

    	//把 ReferenceCloud 类型克隆成 ccPointCloud 类型
    	ccPointCloud* rangeAnglepc = pc->partialClone(&rangeAnglerc);

    	//判断rangeAnglepc是否为空，即所选范围内是否有点
    	if(!rangeAnglepc)
    	{
        	m_app->dispToConsole("[SAF] Failed to extract the range angle subset.", ccMainAppInterface::ERR_CONSOLE_MESSAGE);
        	return;
    	}
		//计算SAF后点数所占的百分比和SAF过程所花的时间
    	m_app->dispToConsole(QString("[SAF] %1% of scan angle points are filtered").arg((rangeAnglerc.size() * 100.0) / count, 0, 'f', 2), ccMainAppInterface::STD_CONSOLE_MESSAGE);
    	m_app->dispToConsole(QString("[SAF] Timing: %1 s.").arg(timer.elapsed() / 1000.0, 0, 'f', 1), ccMainAppInterface::STD_CONSOLE_MESSAGE);

    	//关闭进度条
    	pDlg.close();
    	QApplication::processEvents();

	//		//[进度条]取消SAF	
	//    if (wasCancelled)
	//    {
	//        m_app->dispToConsole("[SAF] SAF was cancelled", ccMainAppInterface::STD_CONSOLE_MESSAGE);
	//        return;
	//    }

    	//隐藏原始点云
    	pc->setEnabled(false);

    	//添加新的一组DB实体
    	ccHObject* cloudContainer = new ccHObject(pc->getName() + QString("_saf"));

    	//设置新点云并添加到实体
    	rangeAnglepc->setVisible(true);
    	rangeAnglepc->setName("SAF Point Cloud");
    	cloudContainer->addChild(rangeAnglepc);

    	//添加实体到DB树
    	m_app->addToDB(cloudContainer);

    	//刷新
    	m_app->refreshAll();
  	}
  	
## 效果

<img src="http://huihut-img.oss-cn-shenzhen.aliyuncs.com/SAFDemo.jpg" width = "99%" />
  	
## 结语

经过了三篇的学习，终于实现了个完整的插件。

回顾我们学习的路线：**插件框架** -> **数据结构** -> **算法实现**

我们不仅从中学会了CC插件的编写，也学到了QT的pro文件编写、QT界面设计、CC运作流程、点云数据结构等。

而我在学习这个插件编写的过程收获更多，因为我是看代码两个月，写代码两小时，Debug两天（差不多啦~不要纠结为什么222~）

看代码的过程是非常痛苦的，CC里面大量的模板编程思想，接口设计思想，还有去他继承谁爸爸的爸爸……

![](http://huihut-img.oss-cn-shenzhen.aliyuncs.com/biaoqing1.gif)

但是期间确实学到很多，以此作为分享，望共勉！