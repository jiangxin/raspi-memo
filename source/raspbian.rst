安装操作系统
==============

树莓派（Raspberry Pi）使用SD卡作为主存储，需要将操作系统刷到SD卡中。在官网\
（http://www.raspberrypi.org/downloads）上提供了不同的操作系统选择。

Raspbian
---------

Raspbian = Raspberry pi + Debian

即是一款基于Debian，针对树莓派定制的Linux发行版。

初始安装
++++++++++

首先就是刷SD卡。下载Raspbin安装包，解开压缩包，使用\ ``dd``\ 命令将镜像写入SD卡。\
在写SD卡时的注意事项：

1. Raspbian安装镜像中包含两个分区（一个挂载到\ ``/boot``\ 卷，一个mount到根卷），因此在使用\
   ``dd``\ 命令时，不要写入SD卡的分区中。即写入\ ``/dev/sdX``\ 、\ ``/dev/diskN``\ ，而非\ ``/dev/sdX1``\ 或\ ``/dev/diskNs1``\ 。

2. Mac OS X，直接写入raw设备速度更快。即写入设备\ ``/dev/rdiskN``\ 。

然后启动树莓派，用如下账号登录：

    * 用户名：pi
    * 口令：raspberry

初次登录进入\ ``raspi-config``\ 配置界面。执行菜单项第一项“1 Expand Filesystem”以扩大根分区空间，\
否则只利用到SD卡的部分空间。

设置显卡内存占用
++++++++++++++++++

Raspberry Pi B型拥有512MB内存，以PoP工艺安装在ARM11（CPU）之上。内存的一部分还要划分给GPU（显卡）使用。\
建议如果有播放视频的需求，要为GPU划分64MB或更多。

划分显卡内存也是使用\ ``raspi-config``\ 配置界面。

::

  $ sudo raspi-config

然后选择“Advanced Options”中的“Memory Split”，输入显卡占用内存大小，默认64MB。

更新Raspbian
+++++++++++++

更改Raspbian的地址记录在APT源文件中。初始的源文件在文件\ ``/etc/apt/sources.list``\ 中：

::

  deb http://mirrordirector.raspbian.org/raspbian/ wheezy main contrib non-free rpi     

将其更改为国内的镜像源，如下：

::

  deb http://mirrors.sohu.com/raspbian/raspbian/ wheezy main contrib non-free rpi

执行如下命令更新 Raspbian：

::

  $ sudo aptitude udpate
  $ sudo aptitude upgrade

更新树莓派固件
++++++++++++++++

升级树莓派固件，使用命令：

::

  $ sudo rpi-update

如果没有找到该命令，则执行如下命令安装：

::

  $ sudo aptitude install rpi-update

参见：\ https://github.com/Hexxeh/rpi-update\ 。

设置中文locale
++++++++++++++++
在\ ``raspi-config``\ 应用的菜单中选择，或者执行下面命令添加中文locale。

::

  $ sudo dpkg-reconfigure locales


安装中文支持
+++++++++++++++++++++
安装\ ``zhcon``\ ，如下：

::

  $ sudo aptitude install zhcon
  $ zhcon --utf8 --drv=vga

然后就能在控制台下显示中文了。

安装中文输入法：

::

  $ sudo aptitude install scim scim-tables-zh scim-pinyin

安装中文字体：

::

  $ sudo aptitude install fonts-arphic-ukai fonts-arphic-uming \
    fonts-arphic-bkai00mp fonts-arphic-bsmi00lp fonts-arphic-gbsn00lp \
    fonts-arphic-gkai00mp ttf-wqy-microhei ttf-wqy-zenhei xfonts-wqy