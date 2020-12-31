---

title: Linux安装配置Shadowsocks客户端及开机自动启动
subtitle: Linux安装配置Shadowsocks客户端及开机自动启动
date: 2017-08-25 09:17:40
author: 辉哈
tags:
	- Shadowsocks
categories: 
	- CS
	
---

## 前言

* Shadowsocks无GUI客户端教程：[Linux安装配置Shadowsocks客户端及开机自动启动](https://blog.huihut.com/2017/08/25/LinuxInstallConfigShadowsocksClient/)
* Shadowsocks-Qt5客户端教程：[Shadowsocks-Qt5 For Centos 7](https://blog.huihut.com/2017/03/25/Shadowsocks-Qt5ForCentos7/)
* Shadowsocks服务端教程：[搬瓦工Shadowsocks安装及配置多用户(服务端)](https://blog.huihut.com/2016/12/03/BandwagonShadowsocksServer/)

## 安装

### Debian/Ubuntu:

    apt-get install python-pip
    pip install shadowsocks

### CentOS:

    sudo yum install python-setuptools && easy_install pip
    sudo pip install shadowsocks
    
<!-- more -->

## 配置

找个地方放shadowsocks的配置文件，一般放到 `/etc`下面：

    sudo vi /etc/shadowsocks.json

我放在我的用户目录下，因为有时需要修改，放在这里方便些：

    vi /home/xx/Software/ShadowsocksConfig/shadowsocks.json

你可以根据自身情况考虑。

然后在`shadowsocks.json`里面添加配置信息，如：

    {
      "server":"my_server_ip",
      "local_address": "127.0.0.1",
      "local_port":1080,
      "server_port":my_server_port,
      "password":"my_password",
      "timeout":300,
      "method":"aes-256-cfb"
    }

把

* `my_server_ip`改为自己的服务器IP
* `my_server_port`改为自己的服务器端口
* `my_server_password`改为自己的密码
* `method`的值改为自己的加密方式，一般是`aes-256-cfb`或者`rc4-md5`


详细配置说明：

|    Name       	| 说明 	|
| ----------    	| ----------- 	|
| server        	|  服务器地址，填ip或域名 |
| local_address   	|  本地地址 	|
| local_port	 	|  本地端口，一般1080，可任意|
| server_port    	|  服务器对外开的端口	|
| password      	|  密码，可以每个服务器端口设置不同密码 |
| port_password    	|  server_port + password ，服务器端口加密码的组合 |
| timeout       	|  超时重连	|
| method        	|  默认: “aes-256-cfb”，见 [Encryption](https://github.com/shadowsocks/shadowsocks/wiki/Encryption)	|
| fast_open     	|  开启或关闭 [TCP_FASTOPEN](https://github.com/shadowsocks/shadowsocks/wiki/TCP-Fast-Open), 填true / false，需要服务端支持|

保存退出就配置好啦！

## 设置代理

### 系统代理

可以选择系统代理，就如下图配置就好啦：

![](http://huihut-img.oss-cn-shenzhen.aliyuncs.com/SystemNetworkProxy.png)

但是系统代理是全局走代理的，访问国内网站一般会有限制（速度较慢、浪费流量、版权受限等）。

所以推荐下面的用浏览器按照规则选择性代理。

### 浏览器代理

#### 安装 SwitchyOmega 插件

以 Chrome 为例，安装 SwitchyOmega 插件代理。

Github 下载 SwitchyOmega：<https://github.com/FelisCatus/SwitchyOmega/releases/>

Chrome 打开[chrome://extensions/](chrome://extensions/)，把插件托进去安装。

#### 配置 Proxy

* `Server`填写`shadowsocks.json`配置中的`local_address`
* `Port`填写`shadowsocks.json`配置中的`local_port`
* 左边`Apply changes`保存。

![](http://huihut-img.oss-cn-shenzhen.aliyuncs.com/SwitchyOmegaProxy.png)

#### 配置 Auto Switch

* `Rule list rules`的`Profile`填`proxy`
* `Default`的`Profile`填`[Direct]`
* `Rule List Format`选择`AutoProxy`
* `Rule List URL`填写`gfwlist`的规则: 
	
		https://raw.githubusercontent.com/gfwlist/gfwlist/master/gfwlist.txt

* 下载规则文件` Download Profile Now`
* 左边`Apply changes`保存

![](http://huihut-img.oss-cn-shenzhen.aliyuncs.com/SwitchyOmegaAutoSwitch.png)

#### 启用 SwitchyOmega

启用 SwitchyOmega 插件，选择 Auto Switch 模式就可以了。

## 测试启动

配置文件的路径改成自己的，如：`/etc/shadowsocks.json`

* 前端启动：`sslocal -c /home/xx/Software/ShadowsocksConfig/shadowsocks.json`；
* 后端启动：`sslocal -c /home/xx/Software/ShadowsocksConfig/shadowsocks.json -d start`；
* 后端停止：`sslocal -c /home/xx/Software/ShadowsocksConfig/shadowsocks.json -d stop`；
* 重启(修改配置要重启才生效)：`sslocal -c /home/xx/Software/ShadowsocksConfig/shadowsocks.json -d restart`

## 开机自启

以下使用Systemd来实现shadowsocks开机自启。

    sudo vim /etc/systemd/system/shadowsocks.service

在里面填写如下内容：

    [Unit]
    Description=Shadowsocks Client Service
    After=network.target

    [Service]
    Type=simple
    User=root
    ExecStart=/usr/bin/sslocal -c /home/xx/Software/ShadowsocksConfig/shadowsocks.json

    [Install]
    WantedBy=multi-user.target

把`/home/xx/Software/ShadowsocksConfig/shadowsocks.json`修改为你的`shadowsocks.json`路径，如：`/etc/shadowsocks.json`

配置生效：

    systemctl enable /etc/systemd/system/shadowsocks.service

输入管理员密码就可以了。

现在你可以马上重启试试，或先在后台启动，等下次重启再看看！

