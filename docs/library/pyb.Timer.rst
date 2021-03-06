.. currentmodule:: pyb
.. _pyb.Timer:

Timer类 – 控制内部定时器
======================================

    定时器可用于多种任务。目前，仅实现了最简单的情况：周期性调用函数。

    每个定时器都包含一个以某一比率计数的计数器。其计数的频率为外设时钟频率（Hz为单位）除以定时器预分频器。
    当计数器到达定时器周期时，会触发事件，且计数器重置为0。通过使用回调函数，定时器事件可调用一个Python函数。

    以固定频率切换LED的示例::

        tim = pyb.Timer(4)              # create a timer object using timer 4 使用定时器4创建一个定时器对象
        tim.init(freq=2)                # trigger at 2Hz 以2Hz触发
        tim.callback(lambda t:pyb.LED(1).toggle())

    将命名函数用于回调的示例::

        def tick(timer):                # we will receive the timer object when being called 我们将在被调用时接收定时器对象
            print(timer.counter())      # show current timer's counter value 显示当前定时器的计数器值
        tim = pyb.Timer(4, freq=1)      # create a timer object using timer 4 - trigger at 1Hz 使用定时器4(以1Hz触发)创建一个定时器对象
        tim.callback(tick)              # set the callback to our tick function 将回调设置为tick函数

    更多示例::

        tim = pyb.Timer(4, freq=100)    # freq in Hz 以Hz为单位的频率
        tim = pyb.Timer(4, prescaler=0, period=99)
        tim.counter()                   # get counter (can also set) 获取计数器（也可设置）
        tim.prescaler(2)                # set prescaler (can also get) 设置预分频器（也可获取）
        tim.period(199)                 # set period (can also get) 设置周期（也可获取）
        tim.callback(lambda t: ...)     # set callback for update interrupt (t=tim instance) 为更新中断设置回调（t=tim实例）
        tim.callback(None)              # clear callback 清除回调

    *Note:* 定时器(2)和定时器(3)分别用于PWM设置LED(3)和LED(4)的亮度。但是，若相关LED的亮度设置为介于1至254的值，
    则这些定时器仅配置为PWM。若LED的亮度特性未使用，则这些定时器可用于一般用途。类似地，定时器(5)控制伺服驱动，
    定时器(6)用于定时ADC/DAC读/写。我们建议您在程序中使用其他定时器。

.. only:: port_moxingstm32f4

    定时器可用于多种任务。目前，仅实现了最简单的情况：周期性调用函数。

    每个定时器都包含一个以某一比率计数的计数器。其计数的频率为外设时钟频率（Hz为单位）除以定时器预分频器。
    当计数器到达定时器周期时，会触发事件，且计数器重置为0。通过使用回调函数，定时器事件可调用一个Python函数。

    将命名函数用于回调的示例::

        def tick(timer):                # we will receive the timer object when being called 我们将在被调用时接收定时器对象
            print(timer.counter())      # show current timer's counter value 显示当前定时器的计数器值
        tim = pyb.Timer(4, freq=1)      # create a timer object using timer 4 - trigger at 1Hz 使用定时器4(以1Hz触发)创建一个定时器对象
        tim.callback(tick)              # set the callback to our tick function 将回调设置为tick函数

    更多示例::

        tim = pyb.Timer(4, freq=100)    # freq in Hz 以Hz为单位的频率
        tim = pyb.Timer(4, prescaler=0, period=99)
        tim.counter()                   # get counter (can also set) 获取计数器（也可设置）
        tim.prescaler(2)                # set prescaler (can also get) 设置预分频器（也可获取）
        tim.period(199)                 # set period (can also get) 设置周期（也可获取）
        tim.callback(lambda t: ...)     # set callback for update interrupt (t=tim instance) 为更新中断设置回调（t=tim实例）
        tim.callback(None)              # clear callback 清除回调
    定时器(5)控制伺服驱动，定时器(6)用于定时ADC/DAC的读写。我们建议您在程序中使用其他定时器。

.. only:: port_openmvcam

    定时器可用于多种任务。此时，仅使用最简单的一种：定时调用函数。

    每个定时器都包含一个以特定平频率计时的定时器。其计数速率为外围时钟频率(单位：Hz)除以定时器的预标量。当计数器到达定时周期时，会触发一个事件，计数器就重新设置为0。通过使用这一回调方法，定时器事件可调用一个Python函数。

    以固定频率切换LED的用法示例::

        tim = pyb.Timer(4)              # create a timer object using timer 4  使用定时器4创建一个定时器对象
        tim.init(freq=2)                # trigger at 2Hz 以2Hz触发
        tim.callback(lambda t:pyb.LED(1).toggle())

    在回调中使用命名函数的示例::

        def tick(timer):                # we will receive the timer object when being called 当被调用时，我们会收到定时器对象
            print(timer.counter())      # show current timer's counter value 示当前定时器的计时值
        tim = pyb.Timer(4, freq=1)      # create a timer object using timer 4 - trigger at 1Hz 使用定时器4创建一个定时器对象-以1Hz触发
        tim.callback(tick)              # set the callback to our tick function

    进一步举例::

        tim = pyb.Timer(4, freq=100)    # freq in Hz 频率单位为Hz
        tim = pyb.Timer(4, prescaler=0, period=99)
        tim.counter()                   # get counter (can also set) 获取定时器（设置也可）
        tim.prescaler(2)                # set prescaler (can also get) 设置分频数（获取也可）
        tim.period(199)                 # set period (can also get) 设置周期（获取也可）
        tim.callback(lambda t: ...)     # set callback for update interrupt (t=tim instance) 设置回调以更新中断
        tim.callback(None)              # clear callback 清空回调

    *注意:* 定时器(1)用于摄像头。同样地，定时器(5)控制servo驱动，定时器(6)用于ADC/DAC读取/写入。建议在您的程序中使用其他定时器。

*注意:* 内存无法在回调(中断)过程中分配，因此在回调中引发的异常不能提供大量信息。如何克服这一限制，请参见:`micropython.alloc_emergency_exception_buf` 。


构造函数
------------

.. class:: pyb.Timer(id, ...)

    .. only:: port_pyboard or port_openmvcam or port_moxingstm32f4

       创建一个给定ID的新定时器对象。若给定额外参数，定时器由 ``init(...)`` 初始化。ID可为1-14。

方法
-------

    .. method:: Timer.init(\*, freq, prescaler, period)

       初始化定时器。初始化必须通过频率(单位：Hz)或通过预标量和周期进行::

           tim.init(freq=100)                  # set the timer to trigger at 100Hz 设置定时器在100Hz触发
           tim.init(prescaler=83, period=999)  # set the prescaler and period directly 直接设置分频数与周期

       键值参数:

         - ``freq`` --- 指定定时器的周期性频率。您可以将此视为定时器经过一个完整周期的频率。

         - ``prescaler``  [0-0xffff] – 指定要加载到定时器的PSC中的值。定时器时钟源除以（ ``prescaler + 1`` ）以得出定时器时钟。
         定时器2-7和12-14的时钟源为84Hz(pyb.freq()[2] * 2)，定时器1和8-11的时钟源为168Hz(pyb.freq()[3] * 2)。

         - ``period`` [0-0xffff] 用于定时器1、3、4、6-15。[0-0x3fffffff]用于定时器2和5。
         指定要加载到定时器的ARR中的值。该值决定定时器的周期（即当计数器循环时）。定时器将在 ``period + 1`` 定时器时钟循环后滚动。

         - ``mode`` 可为下列之一:

           - ``Timer.UP`` - 将定时器配置为从0至ARR（默认）
           - ``Timer.DOWN`` - 将定时器配置为从ARR至0。
           - ``Timer.CENTER`` - 将定时器配置为从0至ARR再到0。

         - ``div`` 可为1或2或4。划分定时器时钟，以确定数字滤波器所使用的采样时钟。

         - ``callback`` - 依据Timer.callback()

         - ``deadtime`` - 指定“dead”的数量或在免费通道（两通道须为非活动通道）间转换的无效时间。
            ``deadtime`` - 可为一个介于0-1008间的整数，并满足下列要求：0-128按照步骤1进行，128-256按照步骤2进行，256-512按照步骤8进行，512-1008按照步骤16进行。
            ``deadime`` - 测量以div时钟滴答划分的 ``source_freq`` 滴答。
            ``Deadtime`` - 仅适用于定时器1和8。

        您必须指定频率或周期和分频数。

.. method:: Timer.deinit()

   反初始化定时器。

      禁用回调（以及关联的中断请求）。

   禁用任何通道回调（以及关联的中断请求）。停用定时器，并禁用定时器外围设备。

.. method:: Timer.callback(fun)

       设置定时器触发时所调用的函数。 ``fun`` 是被传递的1参数，即定时器对象。若 ``fun`` 为 ``None`` ，则禁用回调。

.. method:: Timer.channel(channel, mode, ...)

       若只有一个通道被传递，则返回一个先前初始化的通道对象（若无先前通道，则 ``None`` ）。

       另外，初始化并返回一个定时器通道对象。

       每一通道都可配置来进行脉宽调制、输出比较和输入捕捉。所有通道公用同一基本定时器，即共用同一定时器时钟。

       键值参数：

         - ``mode`` 可为下列之一:

           - ``Timer.PWM`` --- 配置PWM模式下的定时器（高电平有效）。
           - ``Timer.PWM_INVERTED`` --- 配置PWM模式下的定时器（低电平有效）。
           - ``Timer.OC_TIMING`` --- 表示未驱动引脚。
           - ``Timer.OC_ACTIVE`` --- 当比较匹配出现时，引脚就被激活（活性取决于极性）。
           - ``Timer.OC_INACTIVE`` --- 当比较匹配出现时，引脚失效。
           - ``Timer.OC_TOGGLE`` --- 当比较匹配出现时，将切换引脚。
           - ``Timer.OC_FORCED_ACTIVE`` --- 强制激活引脚（忽略比较匹配）。
           - ``Timer.OC_FORCED_INACTIVE`` --- 强制使引脚失效（忽略比较匹配）。
           - ``Timer.IC`` --- 配置输入捕捉模式下的定时器。
           - ``Timer.ENC_A`` --- 配置编码器模式下的定时器。定时器只在CH1改变时改变。
           - ``Timer.ENC_B`` --- 配置编码器模式下的定时器。定时器只在CH2改变时改变。
           - ``Timer.ENC_AB`` --- 配置编码器模式下的定时器。定时器只在CH1和CH2改变时改变。

         - ``callback`` - 依据TimerChannel.callback()。

         - ``pin`` 无（默认值）或一个引脚对象。若指定（非默认值），将导致为此定时器通道配置指定引脚的替代函数。若该引脚不支持任何该定时器通道的替代函数，则将引发错误。

       Timer.PWM模式的键值参数:

         - ``pulse_width`` - 决定使用的初始脉宽值。
         - ``pulse_width_percent`` - 决定使用的初始脉宽值百分比。

       Timer.OC模式的键值参数：

         - ``compare`` - 决定比较寄存器的初始值。

         - ``polarity`` 可为下列之一:

           - ``Timer.HIGH`` - 输出为高电平有效
           - ``Timer.LOW`` - 输出为低电平有效

       Timer.IC模式的可选键值参数：

         - ``polarity`` 可为下列之一:

           - ``Timer.RISING`` - 捕捉上升沿。
           - ``Timer.FALLING`` - 捕捉下降沿。
           - ``Timer.BOTH`` - 捕捉上升沿和下降沿。

         注意：捕捉仅在主通道运行，而不适用于免费通道。

       Timer.ENC模式注意事项：

         - 需2个引脚，其中一个或两个需使用引脚API配置为使用适当的定时器AF。
         - 使用timer.counter()方法来读取编码值。
         - 仅在CH1和CH2上运行（而非CH1N或CH2N）。
         - 设置编码模式时忽略通道数。

.. only:: port_pyboard

           PWM Example::

               timer = pyb.Timer(2, freq=1000)
               ch2 = timer.channel(2, pyb.Timer.PWM, pin=pyb.Pin.board.X2, pulse_width=8000)
               ch3 = timer.channel(3, pyb.Timer.PWM, pin=pyb.Pin.board.X3, pulse_width=16000)

.. only:: port_pyboard

           PWM示例::

               timer = pyb.Timer(4, freq=1000)
               ch2 = timer.channel(1, pyb.Timer.PWM, pin=pyb.Pin("P7"), pulse_width=8000)
               ch3 = timer.channel(2, pyb.Timer.PWM, pin=pyb.Pin("P8"), pulse_width=16000)

.. method:: Timer.counter([value])

       获取或设置定时器。

.. method:: Timer.freq([value])

       获取或设置定时器频率（改变分频数与周期）。

.. method:: Timer.period([value])

       获取或设置定时器周期。

.. method:: Timer.prescaler([value])

       获取或设置定时器分频数。

.. method:: Timer.source_freq()

       获取时钟源的频率。

TimerChannel类 — 为定时器建立一个通道
==================================================

定时器通道用来使用定时器生成/捕捉信号。

使用Timer.channel()方法来创建定时器对象。

方法
-------

    .. method:: timerchannel.callback(fun)

       设置定时器触发时调用的函数。``fun`` 是被传递的1参数，即定时器对象。若 ``fun`` 为 ``None`` ，则禁用回调。

    .. method:: timerchannel.capture([value])

       获取或设置与通道相关的获取值。捕捉、比较和脉宽都是同一函数的别名。捕捉是通道在输入捕捉模式下的所使用的逻辑名。

    .. method:: timerchannel.compare([value])

       获取或设置与通道相关的比较值。捕捉、比较和脉宽都是同一函数的别名。比较是通道在输出比较模式下的所使用的逻辑名。

    .. method:: timerchannel.pulse_width([value])

       获取或设置与通道相关的脉宽值。捕捉、比较和脉宽都是同一函数的别名。脉宽是通道在PWM模式下的所使用的逻辑名。

       在边沿对齐模式下， ``period + 1`` 的脉宽与100%的任务周期相对应。在中心对齐模式下， ``period`` 的脉宽与100%的任务周期相对应。

    .. method:: timerchannel.pulse_width_percent([value])

       获取或设置与通道相关的脉宽百分比。该数值（介于1-100间）设置脉冲活动的定时器周期的百分比。该值可为整数或更为准确的浮点值。例如：取值25则设置任务周期的25%。
