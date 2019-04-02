.. currentmodule :: machine
.. _machine.ADC：

ADC类 - 模数转换
=========================================

用法：：

   进口机器

   adc = machine.ADC（）＃创建一个ADC对象
   apin = adc.channel（pin ='GP3'）＃在GP3上创建一个模拟引脚
   val = apin（）＃读取模拟值

构造函数
------------

.. class :: ADC（id = 0，\ *，bits = 12）

   创建与给定引脚关联的ADC对象。
   这样您就可以读取该引脚上的模拟值。
   有关更多信息，请查看`引脚排列和备用功能
   表。 <https://raw.githubusercontent.com/wipy/wipy/master/docs/PinOUT.png>`_

   .. 警告：：

      ADC引脚输入范围为0-1.4V（绝对最大值为1.8V）
      可以承受）。当GP2，GP3，GP4或GP5重新映射到
      ADC模块，1.8 V是最大值。如果这些引脚用于数字模式，
      那么最大允许输入为3.6V。

方法
-------

..方法:: ADC.channel（id，\ *，pin）

   创建一个模拟引脚。如果只给出通道ID，则正确的引脚将为
   被选中。或者，只有销可以通过并且正确
   将选择频道。例子：：

      ＃所有这些都是等效的，并在GP3上启用ADC通道1
      apin = adc.channel（1）
      apin = adc.channel（pin ='GP3'）
      apin = adc.channel（id = 1，pin ='GP3'）

..方法:: ADC.init（）

   启用ADC块。

..方法:: ADC.deinit（）

   禁用ADC块。

class ADCChannel ---从内部或外部源读取模拟值
================================================== =======================

ADC通道可以连接到MCU的内部点或GPIO引脚。
ADC通道使用ADC.channel方法创建。

.. method :: adcchannel（）

   快速读取通道值的方法。

.. method :: adcchannel.value（）

   读取通道值。

.. method :: adcchannel.init（）

   重新初始化（并有效启用）ADC通道。

.. method :: adcchannel.deinit（）

   禁用ADC通道。