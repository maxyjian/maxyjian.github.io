---
layout: post
title: 'ubuntu 上将 Python 工程打包(deb安装包)'
date: 2018-09-01
author: Max
cover: '/assets/img/pythondeb/theme.jpg'
tags: python deb ubuntu
---

## 前言

&emsp; &ensp;我们写好一个 python 工程后常常需要将软件打包,便于将写好的软件安装在其他没有安装工作环境的电脑上。下面介绍一个 python 工程的打包过程,分为两大步骤:

&emsp; &ensp;一、将工程打包生成一个可执行文件;

&emsp; &ensp;二、将可执行文件和工程依赖的所有文件制作成.deb 安装包。

---
## 将工程打包生成一个可执行文件

&emsp; &ensp;首先给系统装个 easy_install, 如果装了的可以跳过这步。

```shell
sudo apt-get install python-setuptools python-dev build-essential
```
&emsp; &ensp;官网上下载 pyinstaller,网址:  [http://www.pyinstaller.org/](http://www.pyinstaller.org/)

&emsp; &ensp;解包进入源码目录:

```shell
unzip PyInstaller-3.2.zip
cd pyinstaller-3.2
```

&emsp; &ensp;将需打包的工程文件夹里面所有需要的文件 ( 包含主函数文件,如 test.py ) 拷贝到当前目录 ( pyinstaller-3.2 )，生成可执行文件，cd 到 pyinstaller 目录 , 执行:



```shell
 python pyinstaller.py test.py
```

 
&emsp; &ensp;将工程里面除了 .py 文件 ( 作用 : 保留源码 ) 外的所有依赖文件(如数据文件)按原来在工程中的目录拷贝到当前目录的 /test/dist/test 下,在其他工作目录下运行可执行文件 ( 如 /usr/test( 绝对路径 )), 看是否可以执行,若不能运行,可能是以下原因:

&emsp; &ensp;(1) 、路径中有汉字;

&emsp; &ensp;(2) 、你的 python 程序中有路径不会随文件目录变化而变化;

&emsp; &ensp;(3) 、依赖文件没有拷贝或拷贝不完整。

---
## 制作成 .deb 安装包
##### 制作打包文件夹
&emsp; &ensp;新建一个文件夹,例如在用户目录下新建 mydeb 文件夹，在 mydeb 文件夹建立如下结构的文件夹和文件。

```shell
    |——mydeb
        |————usr
            |————lib
                |——可执行文件及执行所需依赖文件(安装后,就在你的/usr/lib 生成相应的可执行文件)
            |————share
                |—icons
                    |——deb.png(启动器图标文件生成到/usr/share/icons/)
                |———applications
                    |——deb.desktop(桌面文件生成到/usr/share/applications/)
        |————DEBIAN(大写、用来制作打包文件)
            |————control(描述 deb 包的信息必须的文件)
```

&emsp; &ensp;[TIPS]:deb.desktop 的建立,sudo gedit deb.desktop,下方设置为.desktop 格式,输入如下内容:

```shell
[Desktop Entry]
Name=mydeb                           #这个是程序名称
Comment=制作 deb 的工具 #注释
Exec=/usr/lib/test                   #可执行文件存放的位置
Icon=/usr/share/deb.png    #图标存放的位置
Terminal=false                        #是否使用终端
Type=Application                  #应用类型
X-Ubuntu-Touch=true          #这个暂时我也不知道是什么用的
Categories=Development  #分类
Name[en]=desktop            
```

&emsp; &ensp;[TIPS]:

&emsp; &ensp;1、#的内容都要删除,不要有任何注释等不必要的信息,否则有时会出现启动程序错误。

&emsp; &ensp;2、#文件夹名首尾千万不要出现空格,否则会出错。

&emsp; &ensp;3、Categories 可以取以下值,表示程序的启动快捷方式放在哪个菜单下:

&emsp; &ensp;&emsp; &ensp;&emsp;应用 Application; 

&emsp; &ensp;&emsp; &ensp;&emsp;Network 放在互联网(Internet);

&emsp; &ensp;&emsp; &ensp;&emsp; 办公 Office;

&emsp; &ensp;&emsp; &ensp;&emsp; 图形 Graphics;

&emsp; &ensp;&emsp; &ensp;&emsp; 声音和视 AudioVideo;

&emsp; &ensp;&emsp; &ensp;&emsp; 系统工具 System;

&emsp; &ensp;&emsp; &ensp;&emsp; 编程 Development;

&emsp; &ensp;&emsp; &ensp;&emsp;附件 Utility;

&emsp; &ensp;&emsp; &ensp;&emsp; 影音 AudioVideo;

&emsp; &ensp;&emsp; &ensp;&emsp; 游戏 Game;

&emsp; &ensp;&emsp; &ensp;&emsp; 首选项 Settings(GNOME;GTK;Settings;HardwareSettings;);

&emsp; &ensp;&emsp; &ensp;&emsp; 系统管理 System。

&emsp; &ensp;4、control 文件内容输入相关信息:

```shell
package: mydeb #安装包的名称
version: 1.0.0 #版本
architecture: i386 #平台maintainer: yang #维护者
description: you can description the deb #描述安装包的信息
```
---
##### 打包
&emsp; &ensp;在 mydeb 文件夹的路径上:

```shell
sudo dpkg -b mydeb program-mydeb_1.0.0_i386.deb 
```
&emsp; &ensp;deb 包正确的命名规则 program-name_version_architeture.deb,最好与它们在 control 文件
里对应的语句相同。

##### 安装

```shell
sudo dpkg -i program-mydeb_1.0.0_i386.deb 
```
&emsp; 或者直接双击.deb 文件,会进入软件中心,点击安装即可。

##### 卸载

```shell
sudo dpkg -P mydeb 
```
&emsp; [TIPS]:1、安装好软件后启动器在/usr/share 下的的 applications 中,可以直接启动。

&emsp; &ensp;&emsp; &emsp; 2、本例也许并不是很符合.deb 打包标准,但是可以安装运行。

---
## 备注：


&emsp; &ensp;&emsp; &ensp;参考博客:

 &emsp; &ensp;&emsp; &ensp;&emsp; &ensp;[http://blog.csdn.net/yangbingzhou/article/details/33318625](http://blog.csdn.net/yangbingzhou/article/details/33318625) 
 
&emsp; &ensp;&emsp; &ensp;&emsp; &ensp;[http://blog.csdn.net/linda1000/article/details/12946297](http://blog.csdn.net/linda1000/article/details/12946297) 
