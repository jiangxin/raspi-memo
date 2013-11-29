树莓派
======
`树莓派（Raspberry Pi） <http://www.raspberrypi.org>`_\ 是由英国剑桥的教授们设计的一个迷你电脑，\
初衷是提供给孩子们一个上手学习软件和硬件的“玩具”。它只占用桌面一个信用卡大小的地方，\
它以全裸露印刷电路板形式展现在科技迷面前。

树莓派采用ARM芯片BCM2835（属于BCM2708家族）作为CPU，配以高性能GPU，号称能够流畅对1080P视频解码。\
在 Linux 下查看CPU信息如下：

.. code-block:: sh

  $ cat /proc/cpuinfo 
  processor       : 0
  model name      : ARMv6-compatible processor rev 7 (v6l)
  BogoMIPS        : 2.00
  Features        : swp half thumb fastmult vfp edsp java tls 
  CPU implementer : 0x41
  CPU architecture: 7
  CPU variant     : 0x0
  CPU part        : 0xb76
  CPU revision    : 7
  
  Hardware        : BCM2708
  Revision        : 000e
  Serial          : 000000009bbfe99a

这款原本用于嵌入设备的CPU性能如何呢？根据相关测评数据，相当于九十年代中晚期奔腾II 300MHz的水平。\
指望用树莓派承担起桌面电脑办公、游戏的重任，恐怕是要失望的。不过因为树莓派价格低廉（B型$35美金），\
体积迷你，非常适合学习软件和硬件的试验品。越来越多的用途已经被开发出来，新的应用正等待你自己去发现。

* `巴黎飞屋环游记 <http://www.raspberrypi.org/archives/5201>`_\ 。
* `PiePal：按下按钮下单订购比萨 <http://www.raspberrypi.org/archives/5256>`_\ 。

树莓派接口
----------
树莓派，一个裸露的电脑板，还需要一些配件才能组装成一个迷你电脑。

电源
+++++
树莓派的电源接口和安卓手机一样都是 Micro USB 接口（5V、750mA），绝大多数都能够满足树莓派的功率要求，\
直接拿来做树莓派的电源。其它可以作为电源的有：充电宝、带外接电源的Usb HUB等等。

如果怀疑树莓派供电不足，可以测量TP1和TP2之间的电压，是否在5V上下15%之间。

SD卡
+++++++++++++
树莓派没有硬盘或硬盘接口，它采用SD卡作为主存。关于主存的容量建议8G或更高。考虑到未来的树莓派\
或者其他迷你电脑可能采用TF卡（Micro SD卡），或者有一天树莓派淘汰的存储卡可以被你的手机或相机使用，\
可以考虑选购TF（Micro SD卡），当然还需要一个TF-SD转接卡套（一般买TF卡会赠送）。

读卡器
+++++++++++++
启动树莓派要求在SD卡中预先写入系统，如果电脑没有读写SD卡的接口，就需要购置一个读卡器。

键盘、鼠标
+++++++++++
USB接口的键盘和鼠标。

HDMI线
+++++++
虽然树莓派提供AV接口和电视相连，不过最常用的还是用其HDMI接口和显示器或电视相连。

外壳
+++++++
选购一款塑料外壳，将树莓派保护起来。要选透明材质才能展现树莓派的曼妙身姿哦。

USB Hub
+++++++++
树莓派拥有两个USB接口。如果USB外设（如外接硬盘、USB WIFI）的功率要就较高，或者有更多的USB外设，\
就需要一款配备外部电源接口的USB Hub。

USB Hub不但可以扩展树莓派USB接口，为USB外设提供额外电力，甚至还可以为树莓派自身供电。

无线网卡
+++++++++
树莓派包含了一个RJ45接口的百兆以太网口，如果需要无线网络接入，就需要购置一个USB无线网卡。

蓝牙模块
++++++++++
如果希望连接蓝牙外设（如蓝牙键盘、鼠标、音箱），或者希望用树莓派启用DLNA，就需要一个USB蓝牙模块。

GPIO转接板等
++++++++++++++
硬件开发用GPIO转接板、面包板、万用表等。

树莓派固件
-----------
查看树莓派固件版本，命令如下：

.. code-block:: sh

  $ vcgencmd version
  Nov 28 2013 21:14:32
  Copyright (c) 2012 Broadcom
  version 97d9a116746b859d0ccceef55b6cbd96b801f5a8 (clean) (release)

命令\ ``vcgencmd``\ 的介绍参见：\ http://elinux.org/RPI_vcgencmd_usage\ 。
主要用法如下：

* 查看内存在CPU和GPU间分配：

  .. code-block:: sh

    root@raspberrypi:~# vcgencmd get_mem arm && vcgencmd get_mem gpu
    arm=448M
    gpu=64M

* 查看时钟频率：

  .. code-block:: sh

    root@raspberrypi:~# \
    > for src in arm core h264 isp v3d uart pwm emmc pixel vec hdmi dpi ; do \
    >     echo -e "$src:\t$(vcgencmd measure_clock $src)" ; \
    > done
    arm:    frequency(45)=700000000
    core:   frequency(1)=250000000
    h264:   frequency(28)=0
    isp:    frequency(42)=250000000
    v3d:    frequency(43)=250000000
    uart:   frequency(22)=3000000
    pwm:    frequency(25)=0
    emmc:   frequency(47)=100000000
    pixel:  frequency(29)=154000000
    vec:    frequency(10)=0
    hdmi:   frequency(9)=163682000
    dpi:    frequency(4)=0

* 温度：查看BCM2835核心温度

  .. code-block:: sh

    root@raspberrypi:~# vcgencmd measure_temp
    temp=42.8'C

* 查看解码器是否开启。默认只开启H264、MPG4、MJPG。

  若要开启更多解码器，访问\ `Raspberry Pi Store <http://www.raspberrypi.com>`_\ 。

  .. code-block:: sh

    root@raspberrypi:~# \
    > for codec in H264 MPG2 WVC1 MPG4 MJPG WMV9 ; do \
    >     echo -e "$codec:\t$(vcgencmd codec_enabled $codec)" ; \
    > done
    H264:   H264=enabled
    MPG2:   MPG2=enabled
    WVC1:   WVC1=enabled
    MPG4:   MPG4=enabled
    MJPG:   MJPG=enabled
    WMV9:   WMV9=enabled
