---

title: Centos 7 挂载 NTFS 分区
subtitle: Centos 7 挂载 NTFS 分区
date: 2017-03-29 20:02:35
author: huihut
tags:
	- Linux
categories: 
	- CS
	
---


# NTFS-3G
## 安装

使用 NTFS-3G 实现，可以挂载 NTFS，还可以挂载 HFS+ 等，以下是在 Centos 7 下安装 NTFS-3G 及挂载 NTFS 分区

    wget -O /etc/yum.repos.d/epel.repo http://mirrors.aliyun.com/repo/epel-7.repo

    sudo yum update

    sudo yum install ntfs-3g

## 查看分区

    fdisk -l

<!-- more -->

## 挂载

    # d、e、f改成你想要挂的盘符名字
    mkdir /mnt/windows/d
    mkdir /mnt/windows/e
    mkdir /mnt/windows/f
    
    # d、e、f改成你想要挂的盘符名字，sdb3这些改为要挂的分区
    mount -t ntfs-3g /dev/sdb3 /mnt/windows/d
    mount -t ntfs-3g /dev/sdb4 /mnt/windows/e
    mount -t ntfs-3g /dev/sdb5 /mnt/windows/f
    
## 卸载
    umount /mnt

## 设置开机自动挂载

    sudo vim /etc/fstab
    
    #只读式挂载：
    /dev/sda1 /mnt/windows/c ntfs-3g ro,umask=0222,defaults 0 0
    
    #读写式挂载：
    /dev/sda1 /mnt/windows/c ntfs-3g rw,umask=0000,defaults 0 0
    #或者： 
    /dev/sda1 /mnt/windows/c ntfs-3g defaults 0 0

## 设置后开机无法启动（无法挂载）

网上很多教程都是如上几步就好了~

然而我的无法开机。

如下图：

![](http://huihut-img.oss-cn-shenzhen.aliyuncs.com/centosNTFSError.jpeg)

这样应该需要输入root密码，用root账户登录修复模式，

然后 `vim /etc/fstab` 删除刚刚添加的东西，

`reboot` 就能进入系统了

所以之前之前忙活的都没用了？

![](http://huihut-img.oss-cn-shenzhen.aliyuncs.com/biaoqing1.gif)

后来试了下原来

	mount -t ntfs-3g /dev/sdb3 /mnt/windows/d

这句本身就无法挂载！

然后可以通过

	sudo mount -o ro /dev/sdb3 /mnt/windows/d
	
挂载，但是只能通过终端访问，就是下面这个👇

## 其他问题无法挂载的解决办法

实现了只能在终端访问
    
    #创建挂载点
    mkdir /mnt/windows/d
    mkdir /mnt/windows/e
    mkdir /mnt/windows/f

    #挂载分区
    sudo mount -o ro /dev/sdb3 /mnt/windows/d
    sudo mount -o ro /dev/sdb4 /mnt/windows/e
    sudo mount -o ro /dev/sdb5 /mnt/windows/f

    #添加上面挂载分区到这里面
    sudo vim /etc/rc.d/rc.local 
    
    #更改权限
    chmod +x /etc/rc.d/rc.local


## Thanks 

<http://www.cnblogs.com/gbyukg/archive/2011/11/02/2232343.html>

<http://www.jianshu.com/p/f578b575fcaa>

<http://askubuntu.com/questions/462381/cant-mount-ntfs-drive-the-disk-contains-an-unclean-file-system>

<https://www.techbrown.com/mount-ntfs-file-system-centos-7-rhel-7.shtml>  