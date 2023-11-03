---

title: iTerm2 + OhMyZsh + agnoster + Powerline + solarized = 漂亮的Mac终端
subtitle: 用iTerm2、 Oh My Zsh、 agnoster、 Powerline、 solarized， 打造漂亮的Mac终端
date: 2017-03-11 14:59:45
author: huihut
tags:
	- Mac
categories: 
	- CS
	
---

## 唠叨一下

自从装了黑苹果后一直用着 Mac 自带的终端（Terminal），相比 Windows 的终端根本无法同台竞技！毕竟 Mac 是基于 Unix 嘛~ 对开发友好太多了。

就是下面这个家伙了👇

![](http://huihut-img.oss-cn-shenzhen.aliyuncs.com/mac_terminal_white.jpg)

本来没觉得什么，直到我看到它👇

<!-- more -->


![](http://huihut-img.oss-cn-shenzhen.aliyuncs.com/iterm2_black.png)

颜值碾压，有木有！

所以就着手折腾了……


## iTerm2

iTerm是一个非常好的终端模拟器，官网地址：[http://iterm2.com/](http://iterm2.com/)，需要下载它。

## zsh 
zsh 是一款强大的虚拟终端，是 Oh My Zsh 的爸爸，所以需要先装它。

在命令行输入：

    zsh --version
    
如果输入zsh的当前版本号就说明装好了，一般Mac自带有的

如果没装则需要输入：

    brew install zsh zsh-completions

这是用Homebrew装，需要Mac上有Homebrew，它的网站：[https://brew.sh/](https://brew.sh/)

## Oh My Zsh

Oh My Zsh 是基于 zsh 的一个扩展工具集，它提供了丰富的扩展功能

它可以通过`curl`或者`wget`来安装

* via curl


        sh -c "$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"

* via wget


        sh -c "$(wget https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh -O -)"
    
装好之后就可以换到 agnoster 主题，就是之前看到的那张颜值主题，Oh My Zsh 一般自带有这个主题。

可以看看它的默认主题：

    ls ~/.oh-my-zsh/themes

需要修改主题只需：

    vim ~/.zshrc
    
然后把里面 `ZSH_THEME` 的值改为 `ZSH_THEME="agnoster"`，保存退出

（[点击这里](https://github.com/robbyrussell/oh-my-zsh/wiki/Themes#agnoster)还有各种主题，任君翻牌~）

应用配置：

    chsh -s /bin/zsh
    
重启 iTerm2

然后你会惊喜地发现……

![](http://huihut-img.oss-cn-shenzhen.aliyuncs.com/iTerm2_noPowerline.jpg)

和颜值主题并不一样！

难道官方坑爹？

震惊！oh-my-zsh 竟然做出这种事！

![](http://huihut-img.oss-cn-shenzhen.aliyuncs.com/biaoqing1.gif)

嘛~ 原来少了 Powerline 的字符集

## Powerline

Git 下来安装：

    git clone git@github.com:powerline/fonts.git
    cd fonts
    ./install.sh
    
然后到 iterm2 配置，设置字体为`Roboto Mono for Powerline`：

![](http://huihut-img.oss-cn-shenzhen.aliyuncs.com/powerline.jpg)

这样就可以有颜值图的效果了~

![](http://huihut-img.oss-cn-shenzhen.aliyuncs.com/iterm2_end.jpg)

然而终端下的 vim 编辑器还是一种颜色~

作为一个追求完美的 geek 怎能容许此等瑕疵呢！

所以就继续捣鼓 solarized 配色。

![](http://huihut-img.oss-cn-shenzhen.aliyuncs.com/biaoqing2.gif)

## solarized

Solarized 是个很牛逼的配色项目，主流OS、IDE、Editor都有它影子。

	# git下Solarized 的源码
    git clone git://github.com/altercation/solarized.git
    
    # 进入文件夹
    cd solarized/vim-colors-solarized/colors
    
    #下面可能要管理员权限
    sudo mkdir -p ~/.vim/colors
    sudo cp solarized.vim ~/.vim/colors/
    
    # 创建.vimrc文件
    sudo vim ~/.vimrc
    
    # 把下面这三行复制进去
    syntax enable
    set background=dark
    colorscheme solarized
    
然后保存 .vimrc, 退出

之后打开用 vim 打开文件就是这种效果了：

![](http://huihut-img.oss-cn-shenzhen.aliyuncs.com/iterm_vim_black.jpg)

## Thanks

> [iTerm2](http://iterm2.com/)
> 
> [robbyrussell/oh-my-zsh](https://github.com/robbyrussell/oh-my-zsh)
>
> [powerline/fonts](https://github.com/powerline/fonts)
>
> [altercation/solarized](https://github.com/altercation/solarized)