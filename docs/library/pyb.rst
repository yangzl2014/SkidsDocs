:mod:`pyb` --- 板级功能
=============================================

.. module:: pyb
   :synopsis: 板级功能

``pyb`` 模块包含与插件相关的特定函数。

时间相关函数
----------------------

.. function:: delay(ms)

   延迟给定的毫秒数。

.. function:: udelay(us)

   延迟给定的微秒数。

.. function:: millis()

   插件重置后，返回毫秒数。

   结果通常是一个Micropython小整数(31位有号数)，因此在2^30毫秒(约12.4天)后，这一数值将开始返回负数。

   注意：若 `pyb.stop()` 发布，支持该功能的硬件计数器将在休眠状态期间暂停。这将影响 `pyb.elapsed_millis()` 的结果。


.. function:: micros()

   插件重置后，返回微秒数。

   结果通常是一个Micropython小整数(31位有号数)，因此在2^30微秒(约17.8分钟)后，这一数值将开始返回负数。

   注意：若 `pyb.stop()` 发布，支持该功能的硬件计数器将在休眠状态期间暂停。这将影响 `pyb.elapsed_micros()` 的结果。


.. function:: elapsed_millis(start)

   返回 ``start`` 后消耗的毫秒数。
   这个函数负责计数器换行，且总是返回一个正数。也就是说，该函数可用来测量长约12.4天的周期。

   例::

       start = pyb.millis()
       while pyb.elapsed_millis(start) < 1000:
           # Perform some operation

.. function:: elapsed_micros(start)

   返回 ``start`` 后消耗的微秒数。

   这个函数负责计数器换行，且总是返回一个正数。也就是说，该函数可用来测量长约17.8分钟的周期。

   例::

       start = pyb.micros()
       while pyb.elapsed_micros(start) < 1000:
           # Perform some operation
           pass

复位相关的函数
-----------------------

.. function:: hard_reset()

   以类似于按下外部RESET按钮的方式重置pyboard或OpenMV Cam。

.. function:: bootloader()

   在不使用BOO T*引脚的情况下激活引导加载程序。

.. function:: fault_debug(value)

   启用或禁用硬故障调试。硬故障即发生在底层系统中的严重错误，例如内存访问失效。

   若value参数为 ``False`` ，则板子会在出现硬故障时自动重设。

   若value参数为 ``True`` ，板子出现硬故障时，则将打印寄存器和堆栈追踪，并无限循环LED。

   禁用默认值，即自动重设。

中断相关的函数
---------------------------

.. function:: disable_irq()

   禁用中断请求。返回先前的IRQ状态： ``False`` / ``True`` （分别为禁用/启用IRQs）。这个返回值可被传递，以启用IRQ，使IRQ返回初始状态。

.. function:: enable_irq(state=True)

   启用中断请求。若 ``state`` 为 ``True``（默认值），则启用IRQ。该函数的最广泛应用为传递由 ``disable_irq`` 返回的值，以退出临界区。

电源相关函数
-----------------------

.. only:: port_pyboard

    .. function:: freq([sysclk[, hclk[, pclk1[, pclk2]]]])

       若未给出参数，则返回一个时钟脉冲频率元组：(sysclk, hclk, pclk1, pclk2)。这些对应:

        - sysclk: CPU频率
        - hclk: AHB总线、核心存储器和DMA的频率
        - pclk1: APB1总线频率
        - pclk2: APB2总线频率

       若给定任何参数，则该函数将设置CPU频率；若给出额外参数，则将设置总线。频率单位为赫兹。例：freq(120000000)即设置sysclk（CPU频率）为120MHz。注意：并非支持所有数值，小于或等于给定数值的最大支持频率会被选中。

       支持的sysclk频率为（单位：赫兹）：8, 16, 24, 30, 32, 36, 40, 42, 48, 54, 56, 60, 64, 72, 84, 96, 108, 120, 144, 168。

       Hclk的最大频率为168MHz，其中pclk1为42MHz ，pclk2为84MHz。请勿设置高于该值的频率。

       Hclk、pclk1和pclk2的频率使用预分频器从sysclk频率中获取。支持Hclk的预分频器为：1, 2, 4, 8, 16, 64, 128, 256, 512。支持pclk1和pclk2的预分频器为：1, 2, 4, 8。将选择一个预标量来匹配所请求的频率。

       8MHz的sysclk频率直接使用HSE(外部晶振)，16MHZ直接使用 HSI(内震荡器)。更高频率使用HSE来启动(锁相环路)，以使用PLL的输出

       注意：若您在启用USB时改变频率，USB可能受到损坏。最好在USB设备启动前，在boot.py中改变频率。在低于36MHz的sysclk频率下，USB无法正常工作。

    .. function:: wfi()

       等待内部或外部中断。

       此处执行 ``wfi`` 指令，在任何中断（无论是内部或外部）出现前，该指令将减低MCU的能耗。此时将继续执行该指令。注意：系统节拍每毫秒（1000Hz）中断一次，因此该功能的停滞最长可达1毫秒。


    .. function:: stop()

       将Pyboard或OpenMV Cam设置在睡眠状态。

       该设置将能耗降低到500uA以下。退出睡眠状态需外部中断或实时闹钟。退出睡眠状态后，系统将继续完成因睡眠而中止的任务。

       见 :meth:`rtc.wakeup` 配置一个实时时钟唤醒事件。

    .. function:: standby()

       将Pyboard或OpenMV Cam设置在深度睡眠状态。

       该设置将能耗降低到50uA以下。退出睡眠状态需实时闹钟或在X1/X18的外部中断。退出睡眠状态后，系统进行硬复位。


       见 :meth:`rtc.wakeup` 配置一个实时时钟唤醒事件。

.. only:: port_openmvcam

    .. function:: freq([sysclk[, hclk[, pclk1[, pclk2]]]])

       若未给出参数，则返回一个时钟脉冲频率元组：(sysclk, hclk, pclk1, pclk2)。这些对应:

        - sysclk: CPU频率
        - hclk: AHB总线、核心存储器和DMA的频率
        - pclk1: APB1总线频率
        - pclk2: APB2总线频率

       若给定任何参数，则该函数将设置CPU频率；若给出额外参数，则将设置总线。频率单位为赫兹。例：freq(120000000)即设置sysclk（CPU频率）为120MHz。注意：并非支持所有数值，小于或等于给定数值的最大支持频率会被选中。

       支持的sysclk频率为（单位：赫兹）：8, 16, 24, 30, 32, 36, 40, 42, 48, 54, 56, 60, 64, 72, 84, 96, 108, 120, 144, 168。

       Hclk的最大频率为168MHz，其中pclk1为42MHz ，pclk2为84MHz。请勿设置高于该值的频率。

       Hclk、pclk1和pclk2的频率使用预分频器从sysclk频率中获取。支持Hclk的预分频器为：1, 2, 4, 8, 16, 64, 128, 256, 512。支持pclk1和pclk2的预分频器为：1, 2, 4, 8。将选择一个预标量来匹配所请求的频率。

       8MHz的sysclk频率直接使用HSE(外部晶振)，16MHZ直接使用 HSI(内震荡器)。更高频率使用HSE来启动(锁相环路)，以使用PLL的输出

       注意：若您在启用USB时改变频率，USB可能受到损坏。最好在USB设备启动前，在boot.py中改变频率。在低于36MHz的sysclk频率下，USB无法正常工作。

    .. function:: wfi()

       等待内部或外部中断。

       此处执行 ``wfi`` 指令，在任何中断（无论是内部或外部）出现前，该指令将减低MCU的能耗。此时将继续执行该指令。注意：系统节拍每毫秒（1000Hz）中断一次，因此该功能的停滞最长可达1毫秒。

    .. function:: stop()

       将Pyboard或OpenMV Cam设置在睡眠状态。

       该设置将能耗降低到500uA以下。退出睡眠状态需外部中断或实时闹钟。退出睡眠状态后，系统将继续完成因睡眠而中止的任务。

       See :meth:`rtc.wakeup` to configure a real-time-clock wakeup event.

    .. function:: standby()

       将Pyboard或OpenMV Cam设置在深度睡眠状态。

       该设置将能耗降低到50uA以下。退出睡眠状态需实时闹钟或在X1/X18的外部中断。退出睡眠状态后，系统进行硬复位。

       See :meth:`rtc.wakeup` to configure a real-time-clock wakeup event.

其他功能
-----------------------

.. only:: port_pyboard

    .. function:: have_cdc()

       如果USB作为串行设备连接，则返回True，否则返回False。

       .. note:: 此功能已弃用。使用pyb.USB_VCP().isconnected()来代替。

    .. function:: hid((buttons, x, y, z))

       采用4元组（或列表）并将其发送到USB主机（PC）以发出HID鼠标移动事件的信号。

       .. note:: 此功能已弃用。使用 :meth:`pyb.USB_HID.send()` 来代替。

.. only:: port_pyboard or port_openmvcam

    .. function:: info([dump_alloc_table])

       打印插件信息。

.. only:: port_pyboard

    .. function:: main(filename)

       设置boot.py完成后要运行的主脚本的文件名。 如果未调用此函数，则将执行默认文件main.py。

       只在boot.py中调用此函数才有意义。


.. only:: port_pyboard or port_openmvcam

    .. function:: mount(device, mountpoint, \*, readonly=False, mkfs=False)

       挂载一个块设备并使其作为文件系统的一部分运行。 ``device`` 须为一个提供块协议的对象：

        - ``readblocks(self, blocknum, buf)``
        - ``writeblocks(self, blocknum, buf)``  (任选)
        - ``count(self)``
        - ``sync(self)``  (任选)

       ``readblocks`` 和 ``writeblocks`` 应从设备上的块数量 ``blocknum`` 开始，
       在 ``buf`` 和块设备之间复制数据。 ``buf`` 为一个长度为512的字节数组。
       若 ``writeblocks`` 未定义，则设备为只读的。这两个函数的返回值被忽略。


       ``count`` 应返回设备上可用的块数。 ``Sync`` 若运行，应同步设备上的数据。

       参数 ``mountpoint`` 是在文件系统的根目录下安装设备的位置。该参数应以斜杠开始。

       若 ``readonly`` 为 ``True``，则设备为只读，否则为可读写。

       若 ``mkfs`` 为 ``True`` ，且尚不存在文件系统，则会创建一个新文件系统。

       卸载设备，只需将设备设置为 ``None`` ，并将挂载位置设置为 ``mountpoint`` 。

.. function:: repl_uart(uart)

   在REPL重复之处，获取或设置UART对象。

.. only:: port_pyboard or port_openmvcam

    .. function:: rng()

       返回一个硬件产生的30位随机数值。

.. function:: sync()

   同步所有文件系统。

.. only:: port_pyboard or port_openmvcam

    .. function:: unique_id()

       返回一个12字节的字符串（96位），即MCU的唯一ID。

.. only:: port_pyboard

    .. function:: usb_mode([modestr], vid=0xf055, pid=0x9801, hid=pyb.hid_mouse)

       若无参数调用，则作为字符串返回当前USB。

       若使用给定 ``modestr`` 调用，则尝试设置USB模式。仅在调用pyb.main()前从 ``boot.py`` 中调用才可实现。以下的 ``modestr`` 值被理解:

       - ``None``: 禁用USB
       - ``'VCP'``: 使用VCP接口启用（虚拟COM端口）
       - ``'VCP+MSC'``: 使用VCP和MSC启用（大容量存储设备类）
       - ``'VCP+HID'``: 使用VCP和HID启用（人工接口设备）

       为实现向后兼容， ``'CDC'`` 理解为
       ``'VCP'`` (对于 ``'CDC+MSC'`` 和 ``'CDC+HID'`` 也是类似的).

       ``vid`` 和 ``pid`` 参数允许您指定VID（供应商id）和PID（产品id）。

       若启用HID模式，您可能也需要通过传递 ``hid`` 关键参数来指定HID的具体细节。
       其需要一个（子类、协议、最大数据包长度、轮询间隔、报告描述符）的元组。默认情况下，其将为USB鼠标设置适当值。
       ``pyb.hid_keyboard`` 常量为USB键盘的适当元组。

类
-------

    .. toctree::
       :maxdepth: 1

       pyb.Accel.rst
       pyb.ADC.rst
       pyb.CAN.rst
       pyb.DAC.rst
       pyb.ExtInt.rst
       pyb.I2C.rst
       pyb.LCD.rst
       pyb.LED.rst
       pyb.Pin.rst
       pyb.RTC.rst
       pyb.Servo.rst
       pyb.SPI.rst
       pyb.Switch.rst
       pyb.Timer.rst
       pyb.UART.rst
       pyb.USB_HID.rst
       pyb.USB_VCP.rst
