---

title: 搬瓦工Shadowsocks安装及配置多用户(服务端)
subtitle: 搬瓦工VPS搭建服务端Shadowsocks，外加多用户配置、优化
date: 2016-12-3 14:44:21
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


## 安装Shadowsocks服务

**方案一：**   

搬瓦工VPS为我们准备了Shadowsocks的一键安装，直接在控制面板中有，非常方便。  

1. 只需在左边最下面选择`Shadowsocks Server`     
2. 然后选择`Install Shadowsocks Server`  
3. 等待安装完成后选择`Go Back`
4. 现在可以看到加密协议默认`aes-256-cfb`,端口默认`443` ,密码随机

如果是自己用，到这里就可以使用了，直接在客户端填好这些配置信息就好了。

**方案二：**  

如果不在控制面板上安装或者是在其他没有一键安装的VPS上，可以使用命令安装。  

**Debian/Ubuntu:**  

	apt-get install python-pip
	pip install shadowsocks 

**CentOS:**  

	yum install python-setuptools && easy_install pip
	pip install shadowsocks 


<!-- more -->

## 优化Shadowsocks性能  

1. 在终端通过ssh连上vps（Windows可以用putty连，Mac直接在终端就可以了）  
2. 在终端输入`vi /etc/sysctl.d/local.conf`
创建配置文件  
3. 按`i`插入  
4. 插入以下内容  

		# max open files
		fs.file-max = 1024000
		# max read buffer
		net.core.rmem_max = 67108864
		# max write buffer
		net.core.wmem_max = 67108864
		# default read buffer
		net.core.rmem_default = 65536
		# default write buffer
		net.core.wmem_default = 65536
		# max processor input queue
		net.core.netdev_max_backlog = 4096
		# max backlog
		net.core.somaxconn = 4096

		# resist SYN flood attacks
		net.ipv4.tcp_syncookies = 1
		# reuse timewait sockets when safe
		net.ipv4.tcp_tw_reuse = 1
		# turn off fast timewait sockets recycling
		net.ipv4.tcp_tw_recycle = 0
		# short FIN timeout
		net.ipv4.tcp_fin_timeout = 30
		# short keepalive time
		net.ipv4.tcp_keepalive_time = 1200
		# outbound port range
		net.ipv4.ip_local_port_range = 10000 65000
		# max SYN backlog
		net.ipv4.tcp_max_syn_backlog = 4096
		# max timewait sockets held by system simultaneously
		net.ipv4.tcp_max_tw_buckets = 5000
		# TCP receive buffer
		net.ipv4.tcp_rmem = 4096 87380 67108864
		# TCP write buffer
		net.ipv4.tcp_wmem = 4096 65536 67108864
		# turn on path MTU discovery
		net.ipv4.tcp_mtu_probing = 1

		# for high-latency network
		net.ipv4.tcp_congestion_control = hybla
		# forward ivp4
		net.ipv4.ip_forward = 1
 
 
5. 然后按`Esc`退出编辑，按`shift`+`:`，输入`wq`，回车，就保存退出了。  
6. 配置生效：`sysctl --system`
 

## 配置多用户
如果想多用户使用的话就需要更改配置。    

1. 首先通过ssh连上vps  
2. 在终端输入`vi /etc/shadowsocks.json`创建配置文件  
3. 按`i`插入  
4. 插入以下内容（用户数任意，注意最后一个用户密码后面没有逗号）  

```
{
 	"server":"my_server_ip",  #填入你的IP地址
	"local_address": "127.0.0.1",
	"local_port":1080,
	"port_password": {
  	    "8381": "foobar1",    #端口号，密码
  	    "8382": "foobar2",
   		"8383": "foobar3",
    	"8384": "foobar4"
	},
 	"timeout":300,
	"method":"aes-256-cfb",
 	"fast_open": false
}
```

上面开了四个端口（用户）为8381-8384，密码分别为foobar1-foobar4，你还需要填入你的`IP地址`。

下面是详细配置说明：

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

1. 然后按`Esc`退出编辑，按`shift`+`:`，输入`wq`，回车，就保存退出了。  
2. 建议使用后端启动 
	* 前端启动：`ssserver -c /etc/shadowsocks.json`；  
	* 后端启动：`ssserver -c /etc/shadowsocks.json -d start`；    
	* 停止：`ssserver -c /etc/shadowsocks.json -d stop`；    
	* 重启(修改配置要重启才生效)：`ssserver -c /etc/shadowsocks.json -d restart`   
3. 设置开机启动  
	1. 在终端输入`vi /etc/rc.local`，  
	2. 把里面最后的带有ssserver的一大段默认的代码删除掉，  
	3. 再把`ssserver -c /etc/shadowsocks.json -d start`加进去，  
	4. 按`wq`保存退出。  
4. 到此就配置好了，试试多用户运行吧！

----

## 修改

### 2017年10月31日修改

附上单用户的配置信息：`/etc/shadowsocks.json`

```
{
    "server":"my_server_ip",  
    "server_port":8381,
    "local_address":"127.0.0.1",
    "local_port":1080,
    "password":"foobar1",
    "timeout":300,
    "method":"aes-256-cfb",
    "fast_open":false
}
```
