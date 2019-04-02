运行你的第一个脚本
=========================

让我们直接进入并在pyboard上运行Python脚本。 后
所有，这就是它的全部！

连接你的pyboard
-----------------------

使用micro USB线将pyboard连接到PC（Windows，Mac或Linux）。
电缆只有一种连接方式，所以你不会错。

.. image:: img/pyboard_usb_micro.jpg

当pyboard连接到PC时，它将打开电源并进入启动状态
进程（引导过程）。 绿色LED应亮起半秒钟或
更少，当它关闭时，意味着启动过程已完成。

打开pyboard USB驱动器
-----------------------------

您的PC现在应该识别pyboard。这取决于您的PC类型
关于接下来会发生什么：

   -  ** Windows **：您的pyboard将显示为可移动USB闪存驱动器。
    Windows可能会自动弹出一个窗口，或者您可能需要去那里
    使用Explorer。

    Windows也会看到pyboard有一个串行设备，它会
    尝试自动配置此设备。如果是，请取消该过程。
    我们将在下一个教程中使用串行设备。

   -  ** Mac **：您的pyboard将作为可移动光盘出现在桌面上。
    它可能会被称为“NONAME”。单击它打开pyboard文件夹。

   -  ** Linux **：您的pyboard将显示为可移动媒体。在Ubuntu上
    它将自动挂载并弹出一个带有pyboard文件夹的窗口。
    在其他Linux发行版上，pyboard可以自动挂载，
    或者你可能需要手动完成。在终端命令行中，键入``lsblk``
    查看连接驱动器的列表，然后``mount / dev / sdb1``（替换``sdb1``
    用适当的设备）。您可能需要root才能执行此操作。

好的，所以你现在应该将pyboard连接为USB闪存盘，并且
窗口（或命令行）应显示pyboard驱动器上的文件。

你正在看的驱动器被pyboard称为``/ flash``，应该包含
以下4个文件：

*`boot.py <http://micropython.org/resources/fresh-pyboard/boot.py>`_  - 当pyboard启动时执行此脚本。它设定
    pyboard的各种配置选项。

*`main.py <http://micropython.org/resources/fresh-pyboard/main.py>`_  - 这是包含Python程序的主脚本。
    它在``boot.py``之后执行。

*`README.txt <http://micropython.org/resources/fresh-pyboard/README.txt>`_  - 这里包含一些关于获取的基本信息
    从pyboard开始。

*`pybcdc.inf <http://micropython.org/resources/fresh-pyboard/pybcdc.inf>`_  - 这是一个用于配置串行USB的Windows驱动程序文件
    设备。在下一个教程中有关于此的更多信息

编辑``main.py``
-------------------

现在我们要编写Python程序，打开``main.py``
文件编辑器中的文件。 在Windows上，您可以使用记事本或任何其他编辑器。
在Mac和Linux上，使用您喜欢的文本编辑器。 打开文件即可
看它包含1行::

    # main.py -- put your code here!

该行以＃字符开头，表示它是* comment *。 这样
线条不会做任何事情，你可以写下你的笔记
程序。

让我们在这个``main.py``文件中加2行，使它看起来像这样：

    # main.py -- put your code here!
    import pyb
    pyb.LED(4).on()

我们写的第一行说我们想要使用``pyb``模块。
该模块包含控制功能的所有功能和类
pyboard的。

我们写的第二行打开蓝色LED：它首先得到“LED”
来自``pyb``模块的类，创建LED编号4（蓝色LED），然后
打开它。

重置pyboard
---------------------

要运行这个小脚本，您需要先保存并关闭``main.py``文件，
然后弹出（或卸载）pyboard USB驱动器。 像你一样这样做
普通的USB闪存盘。

安全弹出/卸载驱动器后，您可以进入有趣的部分：
按下pyboard上的RST开关重置并运行脚本。 RST
开关是电路板上USB连接器正下方的小黑色按钮，
在右边缘。

当您按RST时，绿色LED将快速闪烁，然后是蓝色
LED应该打开并保持开启状态。

恭喜！ 您已经编写并运行了第一个MicroPython
程序！