.. currentmodule:: machine
.. _machine.Timer:

定时器类 -- 控制硬件定时器
======================================

硬件定时器处理周期和事件的时间。在MCUs和SoCs中，定时器可能是最为灵活和异构的硬件，其根据模型不同而大有不同。MicroPython的定时器类使用一个给定周期（或经过某些延迟后）定义执行回调的基线操作，并允许特定板来定义更多非标准行为（因此无法移植到其他板上）。

请参阅关于计时器回调的 :ref:`important constraints <machine_callbacks>` 的讨论。

.. note::

    内存不能在IRQ处理器（一个中断）内部分配，所以处理器内部引发的异常无法提供更多信息。如何克服这一限制，请参见
    :func:`micropython.alloc_emergency_exception_buf` 

构造函数
------------

.. class:: Timer(id, ...)

   创建一个具有给定id的新定时器对象。-1id构造一个虚拟定时器（若由一个板支持）。

方法
-------

.. only :: port_wipy

    ..方法:: Timer.init（mode，\ *，width = 16）

       初始化计时器。例：：

           tim.init（Timer.PERIODIC）#periodic 16-bit timer
           tim.init（Timer.ONE_SHOT，width = 32）＃一次性32位定时器

       关键字参数：
       
          - ``mode``可以是以下之一：
         
            - ``Timer.ONE_SHOT``  - 定时器运行一次，直到配置完毕
             频道的期限到期。
            - ``Timer.PERIODIC``  - 定时器在配置时定期运行
             频道的频率。
            - ``Timer.PWM``  - 在引脚上输出PWM信号。

          - ``width``必须是16或32（位）。对于非常低的频率<5Hz
           （或大周期），应使用32位定时器。 32位模式仅可用
           用于``ONE_SHOT``和``PERIODIC``模式。

..方法:: Timer.deinit（）
   反初始化定时器。停止定时器，并禁用定时器外设。

..
only :: port_wipy

    .. method :: Timer.channel（channel，\ **，freq，period，polarity = Timer.POSITIVE，duty_cycle = 0）
    
       如果只传递了一个通道标识符，那么就是先前初始化的通道
       返回对象（如果没有先前的通道，则返回“无”）。

       否则，初始化并返回TimerChannel对象。
       
       操作模式是配置为用于的Timer对象的操作模式
       创建频道。

        - 如果定时器的宽度是16位，则```channel``，那么必须是``TIMER.A``，``TIMER.B``。
         如果宽度是32位，那么**必须是**``TIMER.A | TIMER.B``。

       仅关键字参数：

          - ``freq``以Hz为单位设置频率。
          - ``period``设置以微秒为单位的周期。

         .. 注意：：

            必须给出``freq``或``period``，而不是两者。

          - ``polarity``这适用于``PWM``，并定义占空比的极性
          - ``duty_cycle``仅适用于``PWM``。这是一个百分比（0.00-100.00）。自从WiPy
           不支持浮点数，占空比必须在0-10000范围内指定，
           其中10000代表100.00,5050代表50.50，依此类推。

       .. 注意：：

          当通道处于PWM模式时，相应的引脚会自动分配
          没有必要通过``Pin``类分配引脚的备用功能。针脚
          支持PWM功能如下：

           - 定时器0通道A上的```GP24``。
           - 定时器1通道A上的“GP25”。
           - 定时器2通道B上的``GP9``。
           - 定时器3通道A上的“GP10”。
           - 定时器3通道B上的```GP11``。

.. only :: port_wipy

    class TimerChannel ---为定时器设置通道
    ==================================================

    定时器通道用于使用定时器生成/捕获信号。

    TimerChannel对象是使用Timer.channel（）方法创建的。

    方法
    -------

    .. method :: timerchannel.irq（\ *，trigger，priority = 1，handler = None）

        此回调的行为在很大程度上取决于操作
        定时器通道的模式：

             - 如果mode是``Timer.PERIODIC``，则定期执行回调
              使用配置的频率或周期。
             - 如果mode是``Timer.ONE_SHOT``，则回调执行一次
              配置的计时器到期。
             - 如果mode是``Timer.PWM``，则在达到占空比时执行回调
              周期值。

        被接受的参数是：

             - 中断的``priority``级别。可以取1-7范围内的值。
              值越高表示优先级越高。
             - ``handler``是一个可选的函数，在触发中断时被调用。
             - 当操作模式为“Timer.PERIODIC”时，``````必须是``Timer.TIMEOUT``
              ``Timer.ONE_SHOT``。在模式为“Timer.PWM”的情况下，触发器必须等于
              ``Timer.MATCH``。

        返回一个回调对象。

.. only :: port_wipy

    .. method :: timerchannel.freq（[value]）

       获取或设置定时器通道频率（以Hz为单位）。

    .. method :: timerchannel.period（[value]）

       获取或设置定时器通道周期（以微秒为单位）。

    .. method :: timerchannel.duty_cycle（[value]）

       获取或设置PWM信号的占空比。这是一个百分比（0.00-100.00）。自从WiPy
       不支持浮点数，占空比必须在0-10000范围内指定，
       其中10000代表100.00,5050代表50.50，依此类推。
常量
---------

.. data:: Timer.ONE_SHOT
.. data:: Timer.PERIODIC

   定时器运行模式。
