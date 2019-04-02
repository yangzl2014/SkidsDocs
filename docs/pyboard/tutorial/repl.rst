获取MicroPython REPL提示
=================================

REPL代表Read Evaluate Print Loop，是给出的名称
您可以在pyboard上访问的交互式MicroPython提示。运用
到目前为止，REPL是测试代码和运行命令的最简单方法。
除了在``main.py``中编写脚本之外，您还可以使用REPL。

要使用REPL，您必须连接到pyboard上的串行USB设备。
如何执行此操作取决于您的操作系统。

Windows
-------

您需要安装pyboard驱动程序才能使用串行USB设备。
驱动程序位于pyboard的USB闪存驱动器上，称为“pybcdc.inf”。

要安装此驱动程序，您需要转到设备管理器
对于您的计算机，在设备列表中找到pyboard（它应该有
旁边有一个警告标志，因为它还没有工作），右键单击
在pyboard设备上，选择Properties，然后选择Install Driver。你需要
然后选择手动查找驱动程序的选项（不要使用Windows自动更新），
导航到pyboard的USB驱动器，然后选择它。然后应该安装。
安装完成后，返回“设备管理器”查找已安装的pyboard，
并查看它是哪个COM端口（例如COM4）。
更全面的说明可以在
`Windows上的pyboard指南（PDF）<http://micropython.org/resources/Micro-Python-Windows-setup.pdf>`_。
如果您在安装驱动程序时遇到问题，请参阅本指南。

您现在需要运行终端程序。如果您可以使用超级终端
安装它，或下载免费程序PuTTY：
`putty.exe <http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html>`_。
使用您的串行程序，您必须连接到您在中找到的COM端口
前一步。使用PuTTY，单击左侧面板中的“Session”，然后单击
右侧的“Serial”单选按钮，然后输入您的COM端口（例如COM4）
“串行线”框。最后，单击“打开”按钮。

Mac OS X
--------

打开终端并运行::

    screen /dev/tty.usbmodem*
    
完成后想要退出屏幕时，键入CTRL-A CTRL  -  \\。

Linux
-----

打开终端并运行::

    screen /dev/ttyACM0
    
您也可以尝试``picocom``或``minicom``而不是屏幕。 你可能不得不这样做
对``ttyACM``使用``/ dev / ttyACM1``或更高的数字。 而且，你可能需要给予
你自己获得访问这些设备的正确权限（例如，组``uucp``或``dialout``，
或使用sudo）。

使用REPL提示符
---------------------

现在让我们尝试直接在pyboard上运行一些MicroPython代码。

打开串口程序（PuTTY，screen，picocom等），您可能会看到一个空白
带闪烁光标的屏幕。 按Enter键，您将看到一个
MicroPython提示符，即``>>>``。 让我们确保它正在使用强制性测试::
    >>> print("hello pyboard!")
    hello pyboard!

在上面，你不应该输入``>>>``字符。 他们在那里
表示您应该在提示符后面键入文本。 最后，一次
你已输入文字``print（“hello pyboard！”）``并按Enter键，输出
在你的屏幕上应该看起来像上面那样。

如果您已经了解了一些python，现在可以在这里尝试一些基本命令。

如果其中任何一个不起作用，您可以尝试硬重置或软重置;
见下文。

继续尝试输入其他一些命令。 例如：：

    >>> pyb.LED(1).on()
    >>> pyb.LED(2).on()
    >>> 1 + 2
    3
    >>> 1 / 2
    0.5
    >>> 20 * 'py'
    'pypypypypypypypypypypypypypypypypypypypy'

重置电路板
-------------------

如果出现问题，您可以通过两种方式重置电路板。 首先是按CTRL-D
在MicroPython提示符下执行软复位。 您将看到类似::

    >>> 
    PYB: sync filesystems
    PYB: soft reboot
    Micro Python v1.0 on 2014-05-03; PYBv1.0 with STM32F405RG
    Type "help()" for more information.
    >>>

如果不起作用，您可以按RST执行硬重置（再次开启和关闭）
开关（最靠近电路板上微型USB插座的小黑色按钮）。 这将结束你的
会话，断开您用于连接到pyboard的任何程序（PuTTY，屏幕等）。

如果要进行硬重置，建议先关闭串行程序并弹出/卸载
pyboard驱动器。
