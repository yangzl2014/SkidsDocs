分发包，包管理和部署应用程序
=====================================================================

就像“大”Python一样，MicroPython支持创建“第三方”
包，分发它们，并在每个用户中轻松安装它们
环境。本章讨论如何实现这些操作。
建议您熟悉Python包装。

概观
--------

以下步骤表示创建和使用时的高级工作流程
包：

1. Python模块和包被转换为分发包
   存档，并在Python Package Index（PyPI）上发布。
2.`upip`包管理器可用于安装分发包
   在具有网络功能的“MicroPython端口”上（例如，
   在Unix端口上）。
3.对于没有网络功能的端口，“安装映像”
   可以在Unix端口上准备，并通过转移到设备
   合适的手段。
4.对于低内存端口，安装映像可以冻结为
   字节码转换为MicroPython可执行文件，从而最大限度地减少内存
   存储开销。

以下部分详细介绍了此过程。

分发包
---------------------

Python模块和包可以打包成适合的档案
在系统之间转移，存储在众所周知的位置（PyPI），
并按需下载部署。这些档案被称为
*分发包*（以区别于Python包
（意思是组织Python源代码））。

MicroPython分发包格式是众所周知的tar.gz
格式，但有一些调整。 Gzip压缩器，用作
TAR压缩文件的外部包装器，默认使用32KB字典
size，表示解压缩压缩流，32KB
需要分配连续的内存。这个要求可能不是
在低内存设备上可以满足，可能有总内存可用
少于这个数量，即使不是，也就是这样一个连续的块
由于内存碎片可能很难分配。以适应
这些约束，MicroPython分发包使用Gzip压缩
用字典大小4K，这应该是一个合适的妥协
在能够解压缩的同时仍然实现一些压缩
即使是最小的设备。

除了小压缩字典大小，MicroPython发行版
包还有其他优化，例如从中删除任何文件
安装过程未使用的存档。特别是，
`upip`包管理器在安装过程中不执行``setup.py``
（见下文），因此该档案不包含在档案中。

同时，这些优化使MicroPython发布
包与`CPython`的包管理器不兼容，``pip``。
这不是一个大问题，因为：

1.软件包可以用`upip`安装，然后可以使用
   CPython（如果它们兼容）。
2.在另一个方向，大多数CPython软件包都是
   首先，由于各种原因与MicroPython不兼容，
   依赖于MicroPython未实现的功能。

总而言之，MicroPython分发包档案非常重要
针对MicroPython的目标环境进行了优化，这些环境非常重要
资源受限的设备。


``upip``包管理
------------------------

MicroPython分发包旨在使用安装
`upip`包经理。 `upip`是一个Python应用程序
通常在启用网络的情况下分布（作为冻结的字节码）
`MicroPython端口<MicroPython端口>`。至少，
`upip`可以在`MicroPython Unix port`中找到。

在提供`upip`的任何`MicroPython port`上，可以访问它
以下：：

    进口upip
    upip.help（）
    upip.install（package_or_package_list，[path]）

其中* package_or_package_list *是分发的名称
要安装的软件包，或安装多个此类名称的列表
包。可选的* path *参数指定文件系统
要安装的位置，默认为标准库
位置（见下文）。

安装特定包然后使用它的示例::

    >>> import upip
    >>> upip.install("micropython-pystone_lowmem")
    [...]
    >>> import pystone_lowmem
    >>> pystone_lowmem.main()

请注意Python包的名称和分发名称
它的包装一般不必匹配，而且经常是它们
别。 这是因为PyPI提供了一个中央包存储库
对于所有不同的Python实现和版本，因此
分发包名称可能需要为特定名称空间命名
实现。 例如，来自`micropython-lib`的所有包
遵循以下命名约定：对于名为的Python模块或包
``foo``，分发包名是``micropython-foo``。

对于从OS命令运行MicroPython可执行文件的端口
提示（如Unix端口），`upip`可以（实际上，通常是）
从命令行运行而不是MicroPython自己的REPL。该
对应于上面示例的命令是::

    micropython -m upip -h
    micropython -m upip install [-p <path>] <packages>...
    micropython -m upip install micropython-pystone_lowmem

[TODO: Describe installation path.]


交叉安装包
-------------------------

对于没有本地网络的`MicroPython端口<MicroPython端口>`
功能，推荐的过程是将它们“交叉安装”到一个
“目录图像”使用`MicroPython Unix端口`，然后
通过合适的方法将该图像传送到设备。

安装到目录映像涉及使用``-p``切换到`upip`::

    micropython -m upip install -p install_dir micropython-pystone_lowmem

在此命令之后，包内容（以及每个依赖的内容）
package）将在``install_dir /``子目录中提供。 您
需要传输这个目录的内容（没有
``install_dir /``前缀）到设备，在合适的位置，在哪里
它可以通过Python``import``语句找到（参见讨论
上面的`upip`安装路径）。


交叉安装冷冻包装
---------------------------------------

对于低内存的“MicroPython端口<MicroPython端口>”，这个过程
上一节中描述的并不提供最有效的方法
资源使用，因为包安装在源表单中，
所以需要在每次导入时编译成bytecome。这个汇编
需要RAM，结果字节码也存储在RAM中，减少了
其可用于存储应用程序数据的数量。而且，这个过程
上面要求在设备上存在文件系统，并且最多
资源受限的设备甚至可能没有它。

字节码冻结是一个解决所有问题的过程
上文提到的：

*源代码预编译为字节码并存储。
*字节码存储在ROM中，而不是RAM中。
*冻结包不需要文件系统。

使用冻结的字节码需要构建可执行文件（固件）
对于来自C源代码的给定`MicroPython端口`。所以，
过程是：

1.按照特定端口的说明设置a
   工具链和建立端口。例如，对于ESP8266端口，
   学习``ports / esp8266 / README.md``中的说明并遵循它们。
   确保您可以构建端口并部署生成的端口
   在继续执行下一步之前，可执行文件/固件已成功完成
2.构建`MicroPython Unix端口`并确保它在你的PATH和
   你可以执行``micropython``。
3.切换到port的目录（例如，对于ESP8266，``ports / esp8266 /``）。
4.运行``make clean-frozen``。这一步清理了以前的任何一个
   安装用于冷冻的模块（因此，您需要
   跳过此步骤添加其他模块，而不是启动
   从头开始）。
5.运行``micropython -m upip install -p modules <packages> ...``to
   安装要冻结的包。
6.运行``make clean``。
7.运行``make``。

在此之后，您应该拥有带有模块的可执行文件/固件
里面的字节码，你可以通常的方式部署。

几点说明：

1.上述序列中的步骤5假定分发包
   可以从PyPI获得。如果不是这样，你需要
   手动将Python源文件复制到``modules /``子目录
   端口端口目录。 （注意upip不支持
   从例如安装版本控制存储库）。
2.裸机设备的固件通常有尺寸限制，
   因此添加太多冻结模块可能会溢出它。通常，你
   如果发生这种情况，会收到链接错误。但是，在某些情况下，
   可能会产生一个图像，该图像不能在设备上运行。这样
   案件一般存在漏洞，应进一步报告
   调查。如果你面对这样的情况，作为第一步，
   您可能希望减少包含的冻结模块的数量。

创建分发包
------------------------------

MicroPython的分发包以相同的方式创建
至于CPython或任何其他Python实现，请参阅参考资料
章的结尾。 应该使用Setuptools（而不是distutils），
因为distutils不支持依赖项和其他功能。 “资源
分发“（``sdist``）格式用于包装。后处理
上面讨论过，（以及下一节讨论的预处理）
是通过使用setuptools的自定义``sdist``命令实现的。 因此，包装
用户只是步骤与标准setuptools相同
需要通过传递来覆盖``sdist``命令的实现
适当的参数``setup（）``call ::

    from setuptools import setup
    import sdist_upip

    setup(
        ...,
        cmdclass={'sdist': sdist_upip.sdist}
    )

上面引用的sdist_upip.py模块可以在中找到
`micropython-lib`：
https://github.com/micropython/micropython-lib/blob/master/sdist_upip.py


应用资源
---------------------

除源代码外，完整的应用程序通常也包含在内
例如，数据文件网页模板，游戏图像等。很清楚如何
处理那些手动安装应用程序时 - 你只需要
某些位置的文件系统中的那些数据文件并使用正常
文件访问功能。

从包中部署应用程序时情况不同 - 这个
更先进，流线型和灵活的方式，但也需要更多
访问数据文件的高级方法。这种方法正在治疗
数据文件为“资源”，并抽象出对它们的访问。

Python使用其“setuptools”库支持资源访问
``pkg_resources``模块。 MicroPython遵循其通常的方法，
具体来说，实现该模块功能的子集
``pkg_resources.resource_stream（package，resource）``函数。
想法是应用程序调用此函数，传递一个
资源标识符，是数据文件的相对路径
指定的包（通常是顶级应用程序包）。它
返回一个流对象，可用于访问资源内容。
因此，``resource_stream（）``模拟标准的接口
`open（）`函数。

在实现方面，``resource_stream（）``使用文件操作
基本上，如果分发包安装在文件系统中。
但是，它还支持在没有底层文件系统的情况下运行，
例如如果包被冻结为字节码。但这需要
打包应用程序时创建的额外中间步骤
“Python资源模块”。

该模块的想法是将二进制数据转换为Python字节
对象，并将其放入字典中，由资源名称索引。
使用重写的``sdist``命令自动完成此转换
在上一节中描述。

让我们使用以下示例跟踪整个过程。假设
您的应用程序具有以下结构::

    my_app/
        __main__.py
        utils.py
        data/
            page.html
            image.png

``__main __。py``和``utils.py``应该使用。来访问资源
以下电话::

    import pkg_resources

    pkg_resources.resource_stream(__name__, "data/page.html")
    pkg_resources.resource_stream(__name__, "data/image.png")

您可以像往常一样使用`MicroPython Unix port`进行开发和调试。
什么时候来制作一个分发包，只需使用
从sdist_upip.py模块中覆盖“sdist”命令，如中所述
上一节。

这将创建一个名为``R.py``的Python资源模块，基于
在``MANIFEST``或``MANIFEST.in``文件中声明的文件（任何非``.py``
文件将被视为资源并添加到``R.py``） - 之前
继续正常的包装步骤。

像这样准备，你的应用程序将在部署时同时工作
文件系统和冻结字节码。

如果你想调试``R.py``创建，你可以运行::

    python3 setup.py sdist --manifest-only

或者，您可以使用tools / mpy_bin2res.py脚本
MicroPython发行版，您可以在其中传递路径
到所有资源文件::

    mpy_bin2res.py data/page.html data/image.png

参考
----------

* Python包装用户指南：https：//packaging.python.org/
* Setuptools文档：https：//setuptools.readthedocs.io/
* Distutils文档：https：//docs.python.org/3/library/distutils.html
