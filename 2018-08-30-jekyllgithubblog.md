---
layout: post
title: '利用搬瓦工和shadowsocks实现翻墙'
date: 2018-08-31
author: MAX
cover: '/assets/img/vpsServer/theme.jpg'
tags: VPS shadowsocks
---

## 购买搬瓦工服务器

1、进入[__搬瓦工官网__](https://www.bwh1.net/) ；

2、注册自己的账号；
![图片1]({{ "assets/img/vpsServer/4.png" | absolute_url }}){:height="50%" width="50%"}

3、登录账号，然后进入VPS Hosting根据需求选择要购买的主机，我选择的是10G VPS的Order KVM，19.99刀/年，点击Order KVM后可以看到配置，选择最下面add to cart进入购买；
![图片2]({{ "assets/img/vpsServer/5.png" | absolute_url }}){:height="50%" width="50%"}
4、可以输入优惠码参与可以打折，然后checkout，选择Alipay使用支付宝扫码支付。

---
## 在购买的主机上安装shadowsocks

1、点击client Area，可看到如下界面，进入services---->my services----> kiwiVMControl Panel；
![图片3]({{ "assets/img/vpsServer/6.png" | absolute_url }}){:height="50%" width="50%"}

2、在Main controls中可以看到IP地址和SSH端口，可开机关机；在Root password modifiction中可设置root用户密码，可用于SSH登录；
![图片4]({{ "assets/img/vpsServer/11.png" | absolute_url }}){:height="50%" width="50%"}

3、左下方可看到Shadowsocks server，进入(若没有这个选项，如下图，则进入此网页：[shadowsocks server](https://kiwivm.64clouds.com/main-exec.php?mode=extras_shadowsocks) ）；
![图片5]({{ "assets/img/vpsServer/8.png" | absolute_url }}){:height="15%" width="15%"}

4、一键安装shadowsocks，如图：
![图片6]({{ "assets/img/vpsServer/10.png" | absolute_url }}){:height="30%" width="30%"}

5、然后进行配置，设置加密方式为、服务器端口和密码，如下图，记录下来，若没有running则选择start，running之后就搭建好了。
![图片7]({{ "assets/img/vpsServer/9.png" | absolute_url }}){:height="50%" width="50%"}

---
## 安卓手机翻墙

&emsp; &ensp;下载[__影梭app__](https://m.ddooo.com/softdown/75259.htm) ，安装后进入，服务器写服务器IP地址，远程端口写shadowsocks server port，本地端口不需要改，密码写shadowsocks server password，加密方式要与服务器一致，我的是aes-256-cfb，点击app右上方云朵图标，连接上就可以google了，自测youtobe看视频也无压力(最好不要用全局，不然国内网站也要走服务器，浪费流量还容易被封)。
![图片8]({{ "assets/img/vpsServer/12.png" | absolute_url }}){:height="20%" width="20%"}

---
## windows翻墙

&emsp; &ensp;下载[<__ShadowsocksR-4.7.0-win__](https://github.com/zhoudaxiaa/ss-client) ，解压后进入ShadowsocksR-dotnet4.0.exe，设置好服务器IP，shadowsocks端口、密码以及加密方式，然后确定。
![图片9]({{ "assets/img/vpsServer/13.png" | absolute_url }}){:height="50%" width="50%"}

&emsp; &ensp;界面如下:
![图片10]({{ "assets/img/vpsServer/14.png" | absolute_url }}){:height="50%" width="50%"}

---
## Ubuntu18.04翻墙

#### 安装shadowsocks

```shell
sudo apt-get update
sudo apt-get install python-pip
sudo apt-get install python-m2crypto
sudo pip install shadowsocks
```

#### 运行shadowsocks

&emsp; &ensp;编辑配置文件/etc/shadowsocks.json 我们可以配置如下：
```shell
{
       "server":"96.56.19.78",
       "server_port":493,
       "local_address":"192.168.0.1",
       "local_port":1080,
       "password":"N3Y2YTdlDW",
       "method":"aes-256-cfb"
}
```
&emsp; &ensp;*[TIPS]: 加密方式推荐使用rc4-md5，因为 RC4 比 AES 速度快好几倍，如果用在路由器上会带来显著性能提升。旧的 RC4 加密之所以不安全是因为 Shadowsocks 在每个连接上重复使用 key，没有使用 IV。*

配置完成之后使用配置文件在后台运行：
```shell
  sudo sslocal -c /etc/shadowsocks.json -d -start   #停止为stop
```
如果遇到错误：
```shell
Undefined symbol:EVP_CIPHER_CTX_cleanup
```
修改方法：
```shell
sudo vi /错误提示/python2.7/site-packages/shadowsocks/crypto/openssl.py
```
将```cleanup```改为```reset```（我的是52和111行），保存并退出，再次执行启动命令（若已start先stop），至此，系统代理服务开启了。

#### 配置chrome浏览器

第一步：我们需要下载一个chrome 浏览器的插件 Proxy SwitchyOmega：
安装Proxy SwitchyOmega插件为了能够让chrome自动切换是否使用系统代理，在chrome浏览器 设置–->扩展程序-–>更多扩展程序–->进行搜索。
![图片11]({{ "assets/img/vpsServer/15.png" | absolute_url }}){:height="50%" width="50%"}

第二步：安装完成之后，我们会在浏览器的菜单栏看到一个蓝色环形小图标，如图，点击进入选项：
![图片12]({{ "assets/img/vpsServer/16.png" | absolute_url }}){:height="30%" width="30%"}

第三步：我们按照之前 etc/shadowsocks.json 里面的配置进行相应的配置，如下图所示：在情景模式中选择 proxy，在代理服务器中的代理协议选择 socks5，本地代理服务器:shadowsocks的localIP，代理端口1080。
![图片13]({{ "/assets/img/vpsServer/17.png" | absolute_url }}){:height="50%" width="50%"}

&emsp; &ensp;进入情景模式Autoswitch，进行如下配置：

&emsp; &ensp;&emsp; &ensp;Rule list 的 profile 选 proxy

&emsp; &ensp;&emsp; &ensp;Default  的 profile 选 direct

&emsp; &ensp;&emsp; &ensp;Rule list Format 选择 AutoProxy


&emsp; &ensp;Rule list URL的gfwlist规则：

&emsp; &ensp;&emsp; &ensp;链接:[https://raw.githubusercontent.com/gfwlist/gfwlist/master/gfwlist.txt](https://raw.githubusercontent.com/gfwlist/gfwlist/master/gfwlist.txt  ) 

&emsp; &ensp;下载规则文件Download Profile Now，左边Apply changes保存，完成之后，一定一定要点击下面的应用选项，进行保存，有博主踩过坑。

第四步：开机启动： 

&emsp; &ensp;以下使用Systemd来实现shadowsocks开机自启：
```shell
sudo vim /etc/systemd/system/shadowsocks.service
```

在里面填写如下内容：
```shell
[Unit]
Description=Shadowsocks Client Service
After=network.target
 
[Service]
Type=simple
User=root
ExecStart=/usr/bin/sslocal -c /mydir/shadowsocks.json
 
[Install]
WantedBy=multi-user.target
```
&emsp; &ensp;*[TIPS]把root改为自己的用户名*

&emsp; &emsp;&emsp; &emsp;*把/mydir/shadowsocks.json修改为你的shadowsocks.json路径*

&emsp; &emsp;&emsp; &emsp;*把/usr/bin/sslocal改为你自己sslocal的路径(我的是/usr/local/bin/sslocal*



#### 翻墙测试

&emsp; &ensp;我们现在打开[__谷歌__](www.google.com) ，你会发现打不开，进入Proxy SwitchyOmega，如图:
![图片14]({{ "assets/img/vpsServer/18.png" | absolute_url }}){:height="25%" width="25%"}
&emsp; &ensp;这时候你会发现有一个资源未加载，我们点击它，如图：
![图片15]({{ "assets/img/vpsServer/19.png" | absolute_url }}){:height="30%" width="30%"}
&emsp; &ensp;选择 proxy情景模式，点击添加条件，完成刷新页面即可。

---
## 备注：

&emsp; &ensp;本文非原创，而是参考多篇博客上综合试验而写，主要参考以下博客：

&emsp; &ensp;&emsp; 1、[ http://jiyiren.github.io/2016/10/06/fanqiang/](http://jiyiren.github.io/2016/10/06/fanqiang/) 

&emsp; &ensp;&emsp; 2、[https://blog.csdn.net/HelloZEX/article/details/80806445](https://blog.csdn.net/HelloZEX/article/details/80806445) 

&emsp; &ensp;&emsp;3、[https://segmentfault.com/a/1190000015725355?utm_source=tag-newest](https://segmentfault.com/a/1190000015725355?utm_source=tag-newest) 

&emsp; &ensp;&emsp; 4、[https://blog.csdn.net/killerstranger/article/details/80620171](https://blog.csdn.net/killerstranger/article/details/80620171) 
    