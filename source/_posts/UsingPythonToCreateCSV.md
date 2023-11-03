---

title: Python 生成带标签数据集的 CSV 文件
subtitle: Python 生成 CSV 文件，可用于生成带标签的数据集 CSV 文件，标签从0开始自动升序：0,1,2,3...
date: 2018-06-17 23:48:32
author: huihut
tags:
	- Python 
	- ML 
categories: 
	- CS
	
---


```python
#!/usr/bin/python
# -*- coding: UTF-8 -*-

# Python 生成 CSV 文件
# Python 生成 CSV 文件，可用于生成带标签的数据集 CSV 文件，标签从0开始自动升序：0,1,2,3...
# 作者：huihut
# 仓库：https://gist.github.com/huihut/9881c98a1d9279d4fa9dfd8475e3fe4b
# 参考：https://github.com/opencv/opencv_attic/blob/master/opencv/modules/contrib/doc/facerec/src/create_csv.py

'''

使用脚本：
* python create_csv.py <base_path> [save_path]
例如：
* python create_csv.py /Users/xx/code/dataset
* python create_csv.py /Users/xx/code/dataset ./dataset_csv.txt

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

生成的 CSV 文件内容（使用 cat 命令查看 dataset_csv.txt 文件内容）：
xx@xxs-MacBook-Pro:~/code/dataset$ cat dataset_csv.txt
/Users/xx/code/dataset/s01/01.pgm;0
/Users/xx/code/dataset/s01/02.pgm;0
...
/Users/xx/code/dataset/s01/10.pgm;0
/Users/xx/code/dataset/s02/01.pgm;1
/Users/xx/code/dataset/s02/02.pgm;1
...
/Users/xx/code/dataset/s10/01.pgm;9
/Users/xx/code/dataset/s10/02.pgm;9
...
/Users/xx/code/dataset/s10/10.pgm;9

'''

import sys
import os.path

if __name__ == "__main__":
    
    SAVE_PATH = "./dataset_csv.txt"

    if (len(sys.argv) != 2 and len(sys.argv) != 3):
        print "usage:"
        print "* python create_csv.py <base_path> [save_path]"
        print "example:"
        print "* python create_csv.py /Users/xx/code/dataset"
        print "* python create_csv.py /Users/xx/code/dataset ./dataset_csv.txt"
        sys.exit(1)
    elif (len(sys.argv) == 3):
        SAVE_PATH = sys.argv[2]
    
    BASE_PATH = sys.argv[1]
    SEPARATOR = ";"
    fh = open(SAVE_PATH,'w')

    label = 0
    for dirname, dirnames, filenames in os.walk(BASE_PATH):
        for subdirname in dirnames:
            subject_path = os.path.join(dirname, subdirname)
            for filename in os.listdir(subject_path):
                abs_path = "%s/%s" % (subject_path, filename)
                print "%s%s%d" % (abs_path, SEPARATOR, label)
                fh.write(abs_path + SEPARATOR + str(label) + "\n")
            label = label + 1
    fh.close()
```