---

title: python-dev 库缺失导致安装 matplotlib 出错
subtitle: python-dev 库缺失导致安装 matplotlib 出错
date: 2018-10-13 11:00:00
author: huihut
tags:
	- Python
categories: 
	- CS
	
---

## 表现

```
sudo pip install matplotlib
```

安装 matplotlib 时出现以下错误

<!-- more -->

```
Installing collected packages: subprocess32, cycler, backports.functools-lru-cache, pyparsing, kiwisolver, matplotlib
  Running setup.py install for subprocess32 ... error
    Complete output from command /usr/bin/python2 -u -c "import setuptools, tokenize;__file__='/tmp/pip-install-azXKeu/subprocess32/setup.py';f=getattr(tokenize, 'open', open)(__file__);code=f.read().replace('\r\n', '\n');f.close();exec(compile(code, __file__, 'exec'))" install --record /tmp/pip-record-5SM9_6/install-record.txt --single-version-externally-managed --compile:
    /usr/lib64/python2.7/distutils/dist.py:267: UserWarning: Unknown distribution option: 'python_requires'
      warnings.warn(msg)
    running install
    running build
    running build_py
    creating build
    creating build/lib.linux-x86_64-2.7
    copying subprocess32.py -> build/lib.linux-x86_64-2.7
    running build_ext
    running build_configure
    checking for gcc... gcc
    checking whether the C compiler works... yes
    checking for C compiler default output file name... a.out
    checking for suffix of executables...
    checking whether we are cross compiling... no
    checking for suffix of object files... o
    checking whether we are using the GNU C compiler... yes
    checking whether gcc accepts -g... yes
    checking for gcc option to accept ISO C89... none needed
    checking how to run the C preprocessor... gcc -E
    checking for grep that handles long lines and -e... /bin/grep
    checking for egrep... /bin/grep -E
    checking for ANSI C header files... yes
    checking for sys/types.h... yes
    checking for sys/stat.h... yes
    checking for stdlib.h... yes
    checking for string.h... yes
    checking for memory.h... yes
    checking for strings.h... yes
    checking for inttypes.h... yes
    checking for stdint.h... yes
    checking for unistd.h... yes
    checking for unistd.h... (cached) yes
    checking fcntl.h usability... yes
    checking fcntl.h presence... yes
    checking for fcntl.h... yes
    checking signal.h usability... yes
    checking signal.h presence... yes
    checking for signal.h... yes
    checking sys/cdefs.h usability... yes
    checking sys/cdefs.h presence... yes
    checking for sys/cdefs.h... yes
    checking for sys/types.h... (cached) yes
    checking for sys/stat.h... (cached) yes
    checking sys/syscall.h usability... yes
    checking sys/syscall.h presence... yes
    checking for sys/syscall.h... yes
    checking for dirent.h that defines DIR... yes
    checking for library containing opendir... none required
    checking for pipe2... yes
    checking for setsid... yes
    checking whether dirfd is declared... yes
    configure: creating ./config.status
    config.status: creating _posixsubprocess_config.h
    building '_posixsubprocess32' extension
    creating build/temp.linux-x86_64-2.7
    gcc -pthread -fno-strict-aliasing -O2 -g -pipe -Wall -Wp,-D_FORTIFY_SOURCE=2 -fexceptions -fstack-protector-strong --param=ssp-buffer-size=4 -grecord-gcc-switches -m64 -mtune=generic -D_GNU_SOURCE -fPIC -fwrapv -DNDEBUG -O2 -g -pipe -Wall -Wp,-D_FORTIFY_SOURCE=2 -fexceptions -fstack-protector-strong --param=ssp-buffer-size=4 -grecord-gcc-switches -m64 -mtune=generic -D_GNU_SOURCE -fPIC -fwrapv -fPIC -I/usr/include/python2.7 -c _posixsubprocess.c -o build/temp.linux-x86_64-2.7/_posixsubprocess.o
    _posixsubprocess.c:16:20: fatal error: Python.h: No such file or directory
     #include "Python.h"
                        ^
    compilation terminated.
    error: command 'gcc' failed with exit status 1
```

## 原因

无法找到 python 库，`#include "Python.h"` 出错

## 解决

* 使用 apt (Ubuntu, Debian...) 安装
```
sudo apt-get install python-dev   # for python2.x installs
sudo apt-get install python3-dev  # for python3.x installs
```

* 使用 yum (CentOS, RHEL...) 安装
```
sudo yum install python-devel   # for python2.x installs
sudo yum install python34-devel   # for python3.4 installs
```

* 使用 dnf (Fedora...) 安装
```
sudo dnf install python2-devel  # for python2.x installs
sudo dnf install python3-devel  # for python3.x installs
```

* 使用 zypper (openSUSE...) 安装
```
sudo zypper in python-devel   # for python2.x installs
sudo zypper in python3-devel  # for python3.x installs
```

* 使用 apk (Alpine...) 安装
```
sudo apk add python2-dev  # for python2.x installs
sudo apk add python3-dev  # for python3.x installs
```