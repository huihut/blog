---

title: C++ 使用 Qt 生成带标签数据集的 CSV 文件
subtitle: C++ 使用 Qt 生成 CSV 文件，可用于生成带标签的数据集 CSV 文件，标签为图片上一级的文件夹名字。
date: 2018-06-12 21:25:51
author: huihut
tags:
    - QT 
    - ML 
categories: 
	- CS
	
---

```cpp
// C++ 使用 Qt 生成 CSV 文件
// 以下函数实现生成特定类型的 CSV 文件，可用于生成带标签的数据集 CSV 文件，标签为图片上一级的文件夹名字。
// 作者：huihut
// 仓库：https://gist.github.com/huihut/c9f43e276ef7652f0471725482a1e4f6

/*

目录结构（使用 tree 命令查看）：

xx@xxs-MacBook-Pro:~/code/dataset$ tree
.
├── README
├── dataset_csv.txt
├── s01
│   ├── 01.pgm
│   ├── ...
│   └── 10.pgm
├── s02
│   ├── 01.pgm
│   ├── ...
│   └── 10.pgm
...
└── s10
    ├── 01.pgm
    ├── ...
    └── 10.pgm

-----------------------------------------------------------

生成的 CSV 文件内容（使用 cat 命令查看 dataset_csv.txt 文件内容）：

xx@xxs-MacBook-Pro:~/code/dataset$ cat dataset_csv.txt
/Users/xx/code/dataset/s01/01.pgm,s01
/Users/xx/code/dataset/s01/02.pgm,s01
...
/Users/xx/code/dataset/s01/10.pgm,s01
/Users/xx/code/dataset/s02/01.pgm,s02
/Users/xx/code/dataset/s02/02.pgm,s02
...
/Users/xx/code/dataset/s10/01.pgm,s10
/Users/xx/code/dataset/s10/02.pgm,s10
...
/Users/xx/code/dataset/s10/10.pgm,s10

*/


#include <QDir>
#include <QDebug>
#include <QDirIterator>

bool CreateCSV()
{
    // 数据集的基础路径
    QString datasetdir = "/Users/xx/code/dataset";

    // 生成的 CSV 文件的名字
    QString csvName = "dataset_csv.txt";

    // 数据集路径与数据集名字的分隔符
    char separator = ',';
    
    // 数据集路径、数据集名字
    QString datasetPath, datasetName;

    // 文件迭代器：获取指定类型（以下是 pgm、png、jpg 三种类型）的数据集文件
    QDirIterator it(datasetdir, QStringList() << "*.pgm" << "*.png" << "*.jpg", QDir::Files, QDirIterator::Subdirectories);
    if(!it.hasNext())
    {
        qDebug() << "当前路径下数据集为空！\n";
        return false;
    }

    // 创建及打开 CSV 文件
    QFile file(datasetdir + QDir::toNativeSeparators("/") + csvName);
    if (!file.open(QIODevice::WriteOnly | QIODevice::Text))
    {
        qDebug() << "打开 CSV 文件失败！\n";
        return false;
    }
    // 文件写的文本流
    QTextStream csv_ts(&file);

    // 文件迭代器中有指定数据集文件则依次迭代
    while (it.hasNext())
    {
        // 数据集路径
        datasetPath = it.next();
        // 数据集名字
        datasetName = datasetPath.section(QDir::toNativeSeparators("/"), -2, -2);
        // 写入文本流
        csv_ts << datasetPath << separator << datasetName << "\n";
    }
    // 关闭文本流
    file.close();

    return true;
}
```