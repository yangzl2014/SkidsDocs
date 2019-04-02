only :: port_wipy

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
       其中10000代表100.00,5050代表50.50，依此类推。.. currentmodule:: machine
.. _machine.UART:

UART类 - 双工串行通信总线
=============================================

UART实现标准的UART / USART双工串行通信协议。在
物理层由2行组成：RX和TX。沟通单位
是一个字符（不要与字符串混淆），可以是8或9
比特宽。

可以使用::创建和初始化UART对象

    从机器导入UART

    uart = UART（1,9600）#init，给定波特率
    uart.init（9600，bits = 8，parity = None，stop = 1）#init with给定参数

支持的参数在板上有所不同：

Pyboard：比特可以是7,8或9.停止可以是1或2.使用* parity = None *，
仅支持8位和9位。启用奇偶校验后，只有7位和8位
得到支持。

WiPy / CC3200：比特可以是5,6,7,8。停止可以是1或2。

UART对象就像一个`stream`对象，完成了读写操作
使用标准流方法::

    uart.read（10）#read 10个字符，返回一个bytes对象
    uart.read（）＃读取所有可用字符
    uart.readline（）＃读取一行
    uart.readinto（buf）＃读取并存储到给定的缓冲区中
    uart.write（'abc'）＃写3个字符

构造函数
------------

.. class :: UART（id，...）

   构造给定id的UART对象。

方法
-------

.. only :: port_wipy

    ..方法:: UART.init（波特率= 9600，位= 8，奇偶校验=无，停止= 1，\ *，引脚=（TX，RX，RTS，CTS））
    
       使用给定参数初始化UART总线：
    
          - ``baudrate``是时钟频率。
          - ``bits``是每个字符的位数，7,8或9。
          - ``parity``是奇偶校验，``None``，0（偶数）或1（奇数）。
          - ``stop``是停止位的数量，1或2。
          - ``pins``是一个4或2项列表，表示TX，RX，RTS和CTS引脚（按此顺序）。
           如果希望UART以有限的功能运行，则任何引脚都可以是无。
           如果给出RTS引脚，则必须同时给出RX引脚。这同样适用于CTS。
           如果没有给出引脚，则采用默认的TX和RX引脚组和硬件
           流量控制将被禁用。如果引脚=无，则不会进行引脚分配。
.. 
method :: UART.deinit（）

   关闭UART总线。

..方法:: UART.any（）

   返回一个整数，计算可以不读取的字符数
   阻塞。如果没有可用字符且为正数，则返回0
   数字，如果有字符。即使有更多，该方法也可以返回1
   超过一个可供阅读的角色。

   有关可用字符的更复杂查询，请使用select.poll ::

    poll = select.poll（）
    poll.register（uart，select.POLLIN）
    poll.poll（超时）

..方法:: UART.read（[nbytes]）

   读字符。如果指定``nbytes``则最多读取多个字节，
   否则读取尽可能多的数据。

   返回值：包含读入的字节的字节对象。返回``None``
   在超时。

..方法:: UART.readinto（buf [，nbytes]）

   将字节读入``buf``。如果指定了``nbytes``，则最多读取
   那很多字节。否则，最多读取``len（buf）``字节。

   返回值：读取并存储到``buf``或``None``的字节数
   超时。

..方法:: UART.readline（）

   读一行，以换行符结尾。

   返回值：读取的行或超时时的“无”。

..方法:: UART.write（buf）

   将字节缓冲区写入总线。

   返回值：写入的字节数或超时时的“无”。

..方法:: UART.sendbreak（）

   在巴士上发送休息条件。这样可以将总线驱动一段时间
   比正常传输角色所需的时间长。
.. 
only :: port_wipy

    .. method :: UART.irq（trigger，priority = 1，handler = None，wake = machine.IDLE）

       创建在UART上接收数据时触发的回调。

            - ``trigger``只能是``UART.RX_ANY``
            - 中断的``priority``级别。可以取1-7范围内的值。
             值越高表示优先级越高。
            - ``handler``是一个可选的函数，在新字符到达时被调用。
            - ``wake``只能是``machine.IDLE``。

       .. 注意：：

          只要满足以下两个条件中的任何一个，就会调用该处理程序：

               - 已收到8个新角色。
               - 在Rx缓冲区中至少有一个新字符正在等待，并且Rx行已经在
                在1个完整帧的持续时间内保持沉默。

          这意味着当调用处理函数时，将介于1到8之间
          人物等待。

       返回一个irq对象。

    常量
    ---------

    .. data:: UART.RX_ANY

        IRQ trigger sources
