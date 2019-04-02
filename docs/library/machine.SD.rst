.. currentmodule :: machine
.. _machine.SD：

SD级 - 安全数字存储卡
======================================

SD卡类允许配置和启用存储卡
WiPy的模块并自动将其作为“/ sd”作为部分安装
的文件系统。有几种引脚组合可以
用于将SD卡插座连接到WiPy，并且使用的引脚可以
在构造函数中指定。请检查`引脚排列和备用功能
表。 <https://raw.githubusercontent.com/wipy/wipy/master/docs/PinOUT.png>`_ for
有关可以重新映射以与SD卡一起使用的引脚的更多信息。

用法示例::

    从机器导入SD
    进口口
    #clk cmd和dat0引脚必须一起传递
    ＃他们各自的替代功能
    sd = machine.SD（pins =（'GP10'，'GP11'，'GP15'））
    os.mount（sd，'/ sd'）
    ＃做正常的文件操作

构造函数
------------

.. class :: SD（id，...）

   创建SD卡对象。如果是初始化，请参阅``init（）``。

方法
-------

.. method :: SD.init（id = 0，pins =（'GP10'，'GP11'，'GP15'））

   启用S​​D卡。为了初始化卡，给它一个3元组：
   ``（clk_pin，cmd_pin，dat0_pin）``。

..方法:: SD.deinit（）

   禁用SD卡。