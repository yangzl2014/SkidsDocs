.. currentmodule:: pyb
.. _pyb.UART:

UART类 – 双向串行通信总线
=============================================

UART执行标准UART/USART双向串行通信协议。其物理层包括两条线：RX和TX。通信单元为8位或9位宽的字符（勿与字符串字符混淆）。



    UART对象可通过下列方式创建和初始化::

        from pyb import UART

        uart = UART(1, 9600)                         # init with given baudrate
        uart.init(9600, bits=8, parity=None, stop=1) # init with given parameters

    位数可为7、8、9。奇偶性可为None、0（偶）、1（奇）。停止位可为1或2。

    *注意:* 奇偶性为None时，仅支持位数为8和9。启用奇偶性时，仅支持位数为7和8。

UART对象与流对象相似，其读取与写入均使用流对象方法::

    uart.read(10)       # read 10 characters, returns a bytes object 读取10字符，返回一个字节对象
    uart.read()         # read all available characters 读取所有可用字符
    uart.readline()     # read a line 读取一条线
    uart.readinto(buf)  # read and store into the given buffer 读取并存入缓冲区
    uart.write('abc')   # write the 3 characters 写入3个字符

    单个字符可通过下列方法读取/写入::

        uart.readchar()     # read 1 character and returns it as an integer 读取一个字符，并返回其整数形式
        uart.writechar(42)  # write 1 character 写入一个字符

    检查是否有内容有待读取，请使用::

        uart.any()          # returns the number of characters waiting 返回等待的字符数量


    *注意：* 流函数 ``read`` 、 ``write`` 等适用于MicroPython v1.3.4。早期版本请使用 ``uart.send`` 和 ``uart.recv`` 。

构造函数
------------

    .. class:: pyb.UART(bus, ...)

       在给定总线上创建一个UART对象。bus可为1,2,3,4,6。若无额外参数，可创建UART对象，但未进行初始化（
       其设置来自总线的最后一次初始化，若存在的话）。若给定额外参数，则总线初始化。初始化参数请参见 ``init`` 。

       UART总线的物理引脚为:

         - ``UART(4)`` is on ``XA``: ``(TX, RX) = (X1, X2) = (PA0, PA1)``
         - ``UART(1)`` is on ``XB``: ``(TX, RX) = (X9, X10) = (PB6, PB7)``
         - ``UART(6)`` is on ``YA``: ``(TX, RX) = (Y1, Y2) = (PC6, PC7)``
         - ``UART(3)`` is on ``YB``: ``(TX, RX) = (Y9, Y10) = (PB10, PB11)``
         - ``UART(2)`` is on: ``(TX, RX) = (X3, X4) = (PA2, PA3)``

       The Pyboard Lite supports UART(1), UART(2) and UART(6) only. Pins are as above except:

         - ``UART(2)`` is on: ``(TX, RX) = (X1, X2) = (PA2, PA3)``

.. only:: port_moxingstm32f4

    .. class:: pyb.UART(bus, ...)

       在给定总线上创建一个UART对象。bus可为1,2,3,4,6。若无额外参数，可创建UART对象，但未进行初始化（
       其设置来自总线的最后一次初始化，若存在的话）。若给定额外参数，则总线初始化。初始化参数请参见 ``init`` 。

       UART总线的物理引脚为:

         - ``UART(4)``: ``(TX, RX) = (P13, P14) = (PA0, PA1)``
         - ``UART(1)``: ``(TX, RX) = (P24, P23) = (PB6, PB7)``
         - ``UART(6)``: ``(TX, RX) = (P5, P6) = (PC6, PC7)``
         - ``UART(3)``: ``(TX, RX) = (P25, P26) = (PB10, PB11)``
         - ``UART(2)``: ``(TX, RX) = (P15, P19) = (PA2, PA3)``

.. only:: port_openmvcam

    .. class:: pyb.UART(bus, ...)

       在给定总线上创建一个UART对象。bus可为3。若无额外参数，可创建UART对象，但未进行初始化（
       其设置来自总线的最后一次初始化，若存在的话）。若给定额外参数，则总线初始化。初始化参数请参见 ``init`` 。

       在OpenMV Cam M4上, UART总线的物理引脚为：

         - ``UART(3)``: ``(TX, RX) = (P4, P5) = (PB10, PB11)``

       在OpenMV Cam M7上, UART总线的物理引脚为：

         - ``UART(1)``: ``(TX, RX) = (P1, P0) = (PB14, PB15)``
         - ``UART(3)``: ``(TX, RX) = (P4, P5) = (PB10, PB11)``

方法
-------

    .. method:: UART.init(baudrate, bits=8, parity=None, stop=1, \*, timeout=1000, flow=0, timeout_char=0, read_buf_len=64)

       使用给定参数初始化UART总线:

         - ``baudrate`` 为时钟频率。
         - ``bits`` 为每个字符的位数，7、8或9。
         - ``parity`` 为奇偶校验， ``None`` ，0（偶）或1（奇）。
         - ``stop`` 为停止位的数量，1或2
         - ``flow`` 设置流控制类型。可为0、 ``UART.RTS``, ``UART.CTS``
           或 ``UART.RTS | UART.CTS``.
         - ``timeout`` 为等待读取/写入首个字符的超时时长（以毫秒为单位）。
         - ``timeout_char`` 为读取或写入时字符间等待的超时时长（以毫秒为单位）。
         - ``read_buf_len`` 为读取缓冲区的字符长度（0为禁用）。

       若波特率不能设置为期望值的5%以内，此方法将会引发故障。最小波特率是由UART所在总线的频率决定的。
       UART(1)和UART(6)为APB2，其他则在APB1。默认总线频率给定UART(1)和UART(6)的最小波特率为1300，
       其他为650。使用pyb.freq来降低总线频率以获得更低的波特率。

       *注意：*  奇偶校验为None时，仅支持8位和9位。启用奇偶校验时，仅支持7位和8位。

.. only:: port_openmvcam

    .. method:: UART.init(baudrate, bits=8, parity=None, stop=1, \*, timeout=1000, flow=0, timeout_char=0, read_buf_len=64)

       使用给定参数初始化UART总线：

         - ``baudrate`` 为时钟频率。
         - ``bits`` 每个字符的位数，可为7、8、9。
         - ``parity`` 为奇偶性，可为None、0（偶）、1（奇）。
         - ``stop`` 是停止位的数量，可为1或2。
         - ``flow`` 设置流控制类型，可为0、 ``UART.RTS``, ``UART.CTS``, ``UART.RTS | UART.CTS``.
         - ``timeout`` 是以毫秒计的等待首字符的超时时长。
         - ``timeout_char`` 是以毫秒计的等待字符间的超时时长。
         - ``read_buf_len`` 是读取缓冲区的字符长度。

       若不将波特率设定为预期值的5%以内，该方法会引发异常。最小波特率取决于UART所在总线的频率。UART(1)在APB2上，UART(3)在APB1上。默认总线频率对应的UART(1)的最小波特率为1300，其他则为650。使用pyb.freq来降低总线频率，以获取更低的波特率。

       *注意：* 奇偶性为None时，仅支持位数为8和9。启用奇偶性时，仅支持位数为7和8。

.. method:: UART.deinit()

   关闭UART总线。

    .. method:: UART.any()

       返回等待的字节数量（可能为0）。

.. method:: UART.read([nbytes])

   读取字符。若指定 ``nbytes`` ，则最多只能读取该数量的字节。若在缓冲区中可用，立即返回，否则在达到足够字符或超时时间过期时返回。

   .. only:: port_pyboard or port_openmvcam or port_moxingstm32f4

      *注意：* 9位字符的每个字符占2字节， ``nbytes`` 须为偶，字符的数量为 ``nbytes/2``.

      Return value: a bytes object containing the bytes read in.  Returns ``None``
      on timeout.

.. method:: UART.readchar()

   在总线上接收单个字符。

   返回值：整数形式的读取的字符。超时返回-1。

.. method:: UART.readinto(buf[, nbytes])

   将字节读取到 ``buf`` 。若指定 ``nbytes`` ，则最多只能读取该数量的字节。否则最多只能读取 ``len(buf)`` 字节。

   返回值：读取并存储在 ``buf`` 或 ``None`` 超时的字节数。

.. method:: UART.readline()

   读取一行，以换行符结尾。若存这样的一行，立即返回。若超时时间过期，无论是否存在新的一行，都返回所有可用数据。

   返回值：读取的行，或超时的 ``None``（若无可用数据）。

.. method:: UART.write(buf)

   .. only:: port_pyboard or port_openmvcam or port_moxingstm32f4

      将字节的缓冲区写入总线。若字符为7位或8位宽，则每个字节为一个字符。若字符为9位宽，则每个字符（小端模式）为2个字节，且 ``buf`` 须包括偶数个字节。

      返回值：写入的字节数。若超时且未写入任何字节，则返回 ``None`` 。

.. only:: port_pyboard or port_openmvcam or port_moxingstm32f4

    .. method:: UART.writechar(char)

      在总线上写入单个字符。 ``char`` 是要写入的整数。返回值： ``None`` 。CTS流控制是否使用，请参见以下注释。

.. method:: UART.sendbreak()

   在总线上发送一个中断状态。这将使得总线持续13位的低位。

   返回值： ``None`` 。

常量
---------

    .. data:: UART.RTS
    .. data:: UART.CTS

       选择流控制类型。

流控制
------------

    在Pyboards V1和V1.1上， ``UART(2)`` 和 ``UART(3)`` 使用以下引脚支持硬件流控制:

        - ``UART(2)`` 在: ``(TX, RX, nRTS, nCTS) = (X3, X4, X2, X1) = (PA2, PA3, PA1, PA0)``
        - ``UART(3)`` 在 :``(TX, RX, nRTS, nCTS) = (Y9, Y10, Y7, Y6) = (PB10, PB11, PB14, PB13)``

    在Pyboard上，Lite仅 ``UART(2)`` 支持以下引脚上的流控制:

        ``(TX, RX, nRTS, nCTS) = (X1, X2, X4, X3) = (PA2, PA3, PA1, PA0)``

    在以下的段落中，术语“target”指连接到UART的设备。

    当UART的 ``init()`` 函数被调用，且 ``flow`` 设置为 ``UART.RTS`` 和 ``UART.CTS`` 中的一个或两个，
    则相关流控制引脚被配置。 ``nRTS`` 为低电平有效输出， ``nCTS`` 为启用上拉的低电平有效输入。
    为实现流控制，Pyboard 的 ``nCTS`` 信号应连接到目标的 ``nRTS`` ， ``nRTS`` 连接到目标的 ``nCTS`` 。

    CTS: 目标控制Pyboard发送器
    ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

    若启用了CTS流控制，则写入行为如下s:

    若调用了Pyboard的 ``UART.write(buf)`` 函数，且 ``nCTS`` 为 ``False`` 时，
    传输将在任何时段停止。若整个缓冲区未在超时周期内传输，则将导致超时。此方法返回写入的字节数量，
    使用户能够根据需要写入剩余的数据。发生超时事件时，字符将保留在UART中。组成该字符的字节数量将包含在返回值中。

    若在 ``nCTS`` 为 ``False`` 时调用 ``UART.writechar()`` ，
    则此方法将会超时，除非目标即使断言 ``nCTS`` 。若 ``OSError 116`` 超时，
    则将出现故障。目标断言 ``nCTS`` 后，字符将立即被传输。

    RTS: Pyboard控制目标发送器
    ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

    若启用RTS流控制，行为应如下:

    若使用缓冲输入（ ``read_buf_len`` > 0），则传入的字符被缓冲。若缓冲区满，
    则接收的下一个字符将导致 ``nRTS`` 出现 ``False`` ：目标应停止传输。字符从缓冲区中读取时， ``nRTS`` 将恢复 ``True`` 。

    注意： ``any()`` 方法返回缓冲区中的字节数量。假设一个 ``N`` 字节的缓冲区长度。
    若缓冲区满，且又接收到字符，则 ``nRTS`` 将被设置为 ``False`` ，且 ``any()`` 将返回N计数。
    字符被读取时，其他字符将被置于缓冲区中，且将包括在随后 ``any()`` 调用的结果中。

    若未使用缓冲输入（ ``read_buf_len`` == 0），接收下一个字符则将导致 ``nRTS`` 出现 ``False`` ，此状态一直持续到字符被读取。


    在Pyboards V1和V1.1上， ``UART(2)`` 和 ``UART(3)`` 使用以下引脚支持硬件流控制:

        - ``UART(2)`` 在: ``(TX, RX, nRTS, nCTS) = (P15, P19, P14, P13) = (PA2, PA3, PA1, PA0)``
        - ``UART(3)`` 在 :``(TX, RX, nRTS, nCTS) = (P25, P26, P22, P20) = (PB10, PB11, PB14, PB13)``

    在以下的段落中，术语“target”指连接到UART的设备。

    当UART的 ``init()`` 函数被调用，且 ``flow`` 设置为 ``UART.RTS`` 和 ``UART.CTS`` 中的一个或两个，
    则相关流控制引脚被配置。 ``nRTS`` 为低电平有效输出， ``nCTS`` 为启用上拉的低电平有效输入。
    为实现流控制，Pyboard 的 ``nCTS`` 信号应连接到目标的 ``nRTS`` ， ``nRTS`` 连接到目标的 ``nCTS`` 。

    CTS: 目标控制发送器
    ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

    若启用了CTS流控制，则写入行为如下:

    若调用了墨星stm32的 ``UART.write(buf)`` 函数，且 ``nCTS`` 为 ``False`` 时，
    传输将在任何时段停止。若整个缓冲区未在超时周期内传输，则将导致超时。此方法返回写入的字节数量，
    使用户能够根据需要写入剩余的数据。发生超时事件时，字符将保留在UART中。组成该字符的字节数量将包含在返回值中。

    若在 ``nCTS`` 为 ``False`` 时调用 ``UART.writechar()`` ，
    则此方法将会超时，除非目标即使断言 ``nCTS`` 。若 ``OSError 116`` 超时，
    则将出现故障。目标断言 ``nCTS`` 后，字符将立即被传输。

    RTS: 控制目标发送器
    ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

    若启用RTS流控制，行为应如下:

    若使用缓冲输入（ ``read_buf_len`` > 0），则传入的字符被缓冲。若缓冲区满，
    则接收的下一个字符将导致 ``nRTS`` 出现 ``False`` ：目标应停止传输。字符从缓冲区中读取时， ``nRTS`` 将恢复 ``True`` 。

    注意： ``any()`` 方法返回缓冲区中的字节数量。假设一个 ``N`` 字节的缓冲区长度。
    若缓冲区满，且又接收到字符，则 ``nRTS`` 将被设置为 ``False`` ，且 ``any()`` 将返回N计数。
    字符被读取时，其他字符将被置于缓冲区中，且将包括在随后 ``any()`` 调用的结果中。

    若未使用缓冲输入（ ``read_buf_len`` == 0），接收下一个字符则将导致 ``nRTS`` 出现 ``False`` ，此状态一直持续到字符被读取。

.. only:: port_openmvcam

    ``UART(3)`` 支持使用下列引脚的RTS/CTS的硬件流控制：

        - ``UART(3)`` 在 :``(TX, RX, nRTS, nCTS) = (P4, P5, P1, P2) = (PB10, PB11, PB14, PB13)``

    在下文中，术语“target”均指与UART相连的设备。

    当UART的 ``init()`` 方法被调用，且 ``flow`` 被设为 ``UART.RTS`` 和 ``UART.CTS`` 的其中一个或两个，
    则配置相关的流控制引脚。 ``nRTS`` 为低电平有效输出，``nCTS`` 为启用了上拉下的低电平有效输入。为实现流控制，O
    penMV Cam的 ``nCTS`` 信号须与目标设备的 ``nRTS`` 相连，OpenMV Cam的 ``nRTS`` 与目标设备的 ``nCTS`` 相连。

    CTS: 目标设备控制OpenMV Cam发送器
    ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

    若启用了CTS流控制，则写入过程如下：

    若调用OpenMV Cam的 ``uart.write(buf)`` 方法，当 ``nCTS`` 为 ``False`` 时，任何周期的传递都会暂停。
    若整个缓冲区未在超时周期内传输，则会导致超时。该方法返回所写入的字节数，允许用户在需要时写入剩余数据。
    在超时事件中，在 ``nCTS`` 之前，字符将始终留在UART中。组成这一字符的字节将包括在返回值中。

    若 ``nCTS`` 为 ``False`` 时 ``uart.writechar()`` 被调用，除非目标设备及时确定 ``nCTS`` ，否则该方法将超时。
    若超时，则会出现 ``OSError 116`` 问题。目标设备确定 ``nCTS`` 后，字符将立即开始传输。

    RTS：OpenMV Cam控制目标设备的传送器
    ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

    若启用RTS流控制，步骤如下：

    若使用了缓冲输入( ``read_buf_len`` > 0)，则输入的字符就会被缓冲。若缓冲区已满，则下一个接收的字符将导致 ``nRTS`` 发生 ``False`` ：
    目标设备应停止传输。字符从缓冲区中读取时， ``nRTS`` 将变为 ``True`` 。

    注意： ``any()`` 方法返回缓冲区中的字节数量。假定一个缓冲区长度。若缓冲区已满，而接收到新一字节，
     ``nRTS`` 将出现 ``False`` ，且 ``any()`` 将返回 ``N`` 数。当字符被读取时，附加字符将被置于缓冲区中，并将包含在随后的 ``any()`` 调用的结果中。

    若未使用缓冲输入（ ``read_buf_len`` == 0），接收下一个字符将使得 ``nRTS`` 在改字符被读取前出现 ``False`` 。

