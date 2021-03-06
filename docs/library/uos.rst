:mod:`uos` -- 基本“操作系统”服务
===============================================

.. module:: uos
   :synopsis: 基本“操作系统”服务

该模块实现相应 `CPython` 模块的子集，如下所示。更多信息，请参见
|see_cpython_module| :mod:`python:os`.

``uos`` 模块包含用于文件系统访问的函数和 ``urandom`` 函数。

函数
---------

.. function:: chdir(path)

   改变当前目录。

.. function:: getcwd()

   获取当前目录。

.. function:: ilistdir([dir])

   该函数返回一个迭代器，该迭代器将生成与它所列出目录中的条目相对应的3元组。无参数情况下，列出当前目录，否则列出由 *dir* 指定的目录。 

   3元组的形式包括 *（名称、类型、索引节点）* :

    - *name* 为一个字符串（若dir为一个字节对象，则名称为字节）且为条目的名称；
    - *type* 为一个指定条目类型的整数，其中目录为0x4000，常规文件为0x8000；
    - *inode* 为一个与文件的索引节点相对应的整数，而对于没有这种概念的文件系统来说，可能为0。

.. function:: listdir([dir])

   若无参数，则列出当前目录；否则将列出给定目录。

.. function:: mkdir(path)

   创建一个新目录。

.. function:: remove(path)

   删除一个文件。

.. function:: rmdir(path)

   删除一个目录。

.. function:: rename(old_path, new_path)

   重命名文件。

.. function:: stat(path)

   获取文件或目录的状态。

.. function:: statvfs(path)

   获取文件系统的状态。

   按照以下顺序返回一个具有文件系统信息的元组:

        * ``f_bsize`` -- 文件系统块大小
        * ``f_frsize`` -- 碎片大小
        * ``f_blocks`` -- f_frsize单元中fs的大小
        * ``f_bfree`` -- 空闲块的数量
        * ``f_bavail`` -- 非特权用户的免费块数
        * ``f_files`` -- 索引节点的数量
        * ``f_ffree`` -- 空闲索引节点的数量
        * ``f_favail`` -- 非特权用户的免费空闲索引节点的数量
        * ``f_flag`` -- 挂载标志
        * ``f_namemax`` -- 最大文件名长度

   与索引节点相关的参数： ``f_files`` 、 ``f_ffree`` 、 ``f_avail`` 、 ``f_flags`` 参数可能会返回0，因为它们在特定于端口的实现中不可用。

.. function:: sync()

   同步所有文件系统。

.. function:: urandom(n)

   返回一个带有n个随机字节的字节对象，该对象由硬件随机数生成器生成。

.. function:: dupterm(stream_object)

   在传递的类似流的对象上复制或切换MicroPython终端（REPL）。给定对象必须实现 ``readinto()`` 
   和 ``write()`` 方法。若传递 ``None`` ，则先前设置的重定向被取消。
