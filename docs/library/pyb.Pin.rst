.. currentmodule:: pyb
.. _pyb.Pin:

Pin类 – 控制I/O 引脚
=============================

引脚是控制I/O引脚的基本对象。引脚可设置引脚的模式（输入、输出等），并能设置数字逻辑层。对引脚的虚拟控制，请参见ADC类。

应用模式:



    所有板上的引脚都预先定义为pyb.Pin.board.Name::

        x1_pin = pyb.Pin.board.X1

        g = pyb.Pin(pyb.Pin.board.X1, pyb.Pin.IN)

    与板上的引脚相对应的CPU引脚都可作为 ``pyb.cpu.Name`` 使用。CPU引脚的名称即为后跟引脚码的端口字母。
    在PYBv1.0中， ``pyb.Pin.board.X1`` 和 ``pyb.Pin.cpu.A0`` 是同一引脚。

    您也可使用字符串::

        g = pyb.Pin('X1', pyb.Pin.OUT_PP)

    使用者可添加自己的名称::

        MyMapperDict = { 'LeftMotorDir' : pyb.Pin.cpu.C12 }
        pyb.Pin.dict(MyMapperDict)
        g = pyb.Pin("LeftMotorDir", pyb.Pin.OUT_OD)

    也可查询映射::

        pin = pyb.Pin("LeftMotorDir")

    使用者还可添加自己的映射函数::

        def MyMapper(pin_name):
           if pin_name == "LeftMotorDir":
               return pyb.Pin.cpu.A0

        pyb.Pin.mapper(MyMapper)

    所以，若您要调用： ``pyb.Pin("LeftMotorDir", pyb.Pin.OUT_PP)``
    则 ``"LeftMotorDir"`` 直接传递到映射函数。

    简而言之，以下顺序决定了名字如何被映射成一个引脚编号：

    1. 直接指定一个引脚对象
    2. 用户提供映射函数
    3. 用户提供映射（对象必须是可用的关键字）
    4. 提供一个与板上引脚匹配的字符串
    5. 提供一个与CPU端口/引脚匹配的字符串


    您可以设置 ``pyb.Pin.debug(True)`` ，以获得一些关于一个特定对象如何映射到一个引脚的调试信息。

    当引脚启用了 ``Pin.PULL_UP`` 或 ``Pin.PULL_DOWN`` 模式，该引脚有一个有效的40k欧姆的电阻把它牵引至3V3或GND（引脚Y5除外，该引脚电阻为11k欧姆）。

    每当在GPIO引脚中发现下降沿，该回调将被执行。注意：机械按钮有抖动，按下或松开一个开关通常会产生多次电平变化。

    详细解释与消除抖动的具体方法，请参见: http://www.eng.utah.edu/~cs5780/debouncing.pdf

    所有引脚对象都通过引脚映射器来生成一个GPIO引脚。



构造函数
------------

.. class:: pyb.Pin(id, ...)

   创建一个与id关联的新的注脚对象。若给定其他参数，则用来初始化引脚。详见 `pin.init()` 。


    Class methods
    -------------

    .. classmethod:: Pin.debug([state])

       获取或设置调试状态( ``True`` 与 ``False`` 分别对应开和关)。

    .. classmethod:: Pin.dict([dict])

       获取或设置注脚映射库。

    .. classmethod:: Pin.mapper([fun])

       获取或设置注脚映射器函数。


方法
-------


    .. method:: Pin.init(mode, pull=Pin.PULL_NONE, af=-1)

       初始化注脚：

         - ``mode`` 可为下列之一:

            - ``Pin.IN`` - 将引脚设置为输入；
            - ``Pin.OUT_PP`` - 将引脚设置为推挽输出；
            - ``Pin.OUT_OD`` - 将引脚设置为开漏输出；
            - ``Pin.AF_PP`` - 将引脚设置为复用推挽；
            - ``Pin.AF_OD`` - 将引脚设置为复用开漏；
            - ``Pin.ANALOG`` - 将引脚设置为模拟量。

         - ``pull`` 可为下列之一:

            - ``Pin.PULL_NONE`` - 无上拉或下拉电阻；
            - ``Pin.PULL_UP`` - 启用上拉电阻；
            - ``Pin.PULL_DOWN`` - 启用下拉电阻。

         - 当模式为 ``Pin.AF_PP`` 或 ``Pin.AF_OD`` ，AF可为与引脚相关联的一个替代函数的索引或名称。

       返回: ``None``.

.. method:: Pin.value([value])

   获取或设置引脚的数字逻辑级别:

     - 若无参数，返回0或1取决于引脚的逻辑级别。
     - 给定 ``value`` ，设置引脚的逻辑级别。 ``value`` 可为任何可转换成布尔值的值。若转换为 ``True`` ，引脚设置为高；否则设置为低。


    .. method:: Pin.__str__()

       返回一个描述引脚对象的字符串。

    .. method:: Pin.af()

       返回引脚当前配置的替代函数。返回的整数将与用于初始化函数的AF参数的允许常量之一相匹配。

    .. method:: Pin.af_list()

       Returns an array of alternate functions available for this pin.

    .. method:: Pin.gpio()

       返回与这一引脚相关联的GPIO数据块的基地址。

    .. method:: Pin.mode()

       返回引脚的当前配置模式。返回的整数将与将与用于初始化函数的模式参数的允许常量之一相匹配。

    .. method:: Pin.name()

       获取引脚名称。

    .. method:: Pin.names()

       返回用于该引脚的CPU和插件名称。

    .. method:: Pin.pin()

       获取引脚数量。

    .. method:: Pin.port()

       获取引脚端口。

.. method:: Pin.pull()

    返回引脚当前配置的pull。返回的整数将与将与用于初始化函数的pull参数的允许常量之一相匹配。

常量
---------


    .. data:: Pin.AF_OD

       使用开漏驱动来将引脚初始化为复用功能模式。

    .. data:: Pin.AF_PP

       使用推挽式驱动将引脚初始化为复用功能模式

    .. data:: Pin.ANALOG

       将引脚初始化为模拟。

    .. data:: Pin.IN

       将引脚初始化为输入模式。

    .. data:: Pin.OUT_OD

       使用开漏驱动将引脚初始化为输出模式。

    .. data:: Pin.OUT_PP

       使用推挽驱动将引脚初始化为输出模式。

    .. data:: Pin.PULL_DOWN

       在引脚上启用下拉电阻。

    .. data:: Pin.PULL_NONE

       请勿在引脚上启用任何上拉或下拉电阻。

    .. data:: Pin.PULL_UP

       在引脚上启用上拉电阻。


    class PinAF -- 引脚替代函数
    ======================================

    引脚意为微处理器上的物理引脚。每个引脚都可有一系列函数（GPIO、I2C SDA等）。每个AF引脚对象都代表一个引脚的特定函数。

    用法示例::

        x3 = pyb.Pin.board.X3
        x3_af = x3.af_list()

    现在x3_af将包含一个在X3引脚上可用的PinAF对象的数组。

    对于pyboard，x3_af包含:
        [Pin.AF1_TIM2, Pin.AF2_TIM5, Pin.AF3_TIM9, Pin.AF7_USART2]

    通常情况下，每个外设都会自动配置af，但有时统一函数在多个引脚上可用，且需要更多控制。

    配置X3以显示TIM2_CH3，您可使用::

       pin = pyb.Pin(pyb.Pin.board.X3, mode=pyb.Pin.AF_PP, af=pyb.Pin.AF1_TIM2)

    或::

       pin = pyb.Pin(pyb.Pin.board.X3, mode=pyb.Pin.AF_PP, af=1)

    方法
    -------

    .. method:: pinaf.__str__()

       返回一个描述替代函数的字符串。

    .. method:: pinaf.index()

       返回替代函数索引。

    .. method:: pinaf.name()

       返回替代函数的名称。

    .. method:: pinaf.reg()

       返回与分配给此替代函数的外设相关的基址寄存器。例如，若替代函数为TIM2_CH3，则将返回stm.TIM2。
