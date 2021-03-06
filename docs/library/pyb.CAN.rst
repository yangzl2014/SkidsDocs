.. currentmodule:: pyb

CAN类 –控制器区域网络通信总线
======================================================

CAN实施标准CAN通信协议。其物理层包括两道线：RX和TX。注意：将pyboard或OpenMV Cam与CAN总线连接时，您必须使用来CAN传输器将来自的CAN逻辑信号转换为总线上正确的电压等级。

.. only:: port_pyboard or port_moxingstm32f4

    Example usage (works without anything connected)::

        from pyb import CAN
        can = CAN(1, CAN.LOOPBACK)
        can.setfilter(0, CAN.LIST16, 0, (123, 124, 125, 126))  # set a filter to receive messages with id=123, 124, 125 and 126
        can.send('message!', 123)   # send a message with id 123
        can.recv(0)                 # receive message on FIFO 0

.. only:: port_openvmcam

    范例用法（不作任何连接的单独运行）::

        from pyb import CAN
        can = CAN(2, CAN.LOOPBACK)
        can.setfilter(0, CAN.LIST16, 0, (123, 124, 125, 126))  # set a filter to receive messages with id=123, 124, 125 and 126
        can.send('message!', 123)   # send a message with id 123
        can.recv(0)                 # receive message on FIFO 0

构造函数
------------

.. only:: port_pyboard

    .. class:: pyb.CAN(bus, ...)

       在给定总线上构建一个CAN对象。 ``bus`` 可为2。在没有其他参数的情况下，可创建CAN对象，但未进行初始化（
       该对象的设置来自总线的最后一次初始化，若存在的话）。若给定额外参数，则总线进行初始化。初始化参数详见 ``init`` 。

       CAN的物理引脚为：

         - ``CAN(1)`` is on ``YA``: ``(RX, TX) = (Y3, Y4) = (PB8, PB9)``
         - ``CAN(2)`` is on ``YB``: ``(RX, TX) = (Y5, Y6) = (PB12, PB13)``

.. only:: port_moxingstm32f4

    .. class:: pyb.CAN(bus, ...)

       在给定总线上构建一个CAN对象。 ``bus`` 可为2。在没有其他参数的情况下，可创建CAN对象，但未进行初始化（
       该对象的设置来自总线的最后一次初始化，若存在的话）。若给定额外参数，则总线进行初始化。初始化参数详见 ``init`` 。

       CAN的物理引脚为：

         - ``CAN(1)``: ``(RX, TX) = (P9, P8) = (PB8, PB9)``
         - ``CAN(2)``: ``(RX, TX) = (P3, P20) = (PB12, PB13)``

.. only:: port_openmvcam

    .. class:: pyb.CAN(bus, ...)

       在给定总线上构建一个CAN对象。 ``bus`` 可为2。在没有其他参数的情况下，可创建CAN对象，但未进行初始化（
       该对象的设置来自总线的最后一次初始化，若存在的话）。若给定额外参数，则总线进行初始化。初始化参数详见 ``init`` 。

       CAN的物理引脚为：

         - ``CAN(2)``: ``(RX, TX) = (P3, P2) = (PB12, PB13)``

Class Methods
-------------
.. classmethod:: CAN.initfilterbanks(nr)

    .. only:: port_pyboard

       Reset and disable all filter banks and assign how many banks should be available for CAN(1).

       STM32F405 has 28 filter banks that are shared between the two available CAN bus controllers.
       This function configures how many filter banks should be assigned to each. ``nr`` is the number of banks
       that will be assigned to CAN(1), the rest of the 28 are assigned to CAN(2).
       At boot, 14 banks are assigned to each controller.

    .. only:: port_openmvcam

       复位并禁用所有滤波器组，并分配可用于CAN(2)的组。

       ``nr`` 是将被分配到CAN(2)的bank的数量. 启动时, 14个bank分配给CAN(2).

方法
-------

.. method:: CAN.init(mode, extframe=False, prescaler=100, \*, sjw=1, bs1=6, bs2=8)

   使用给定参数初始化CAN总线：

     - ``mode`` 为下列之一：NORMAL, LOOPBACK, SILENT, SILENT_LOOPBACK
     - 若 ``extframe`` 为True，总线使用框架（29位）中的扩展标识符；否则使用标准的11位标识符。
     - ``prescaler`` 用于设置1时间量的持续时长；时间量为分配器分割的输入时钟（PCLK1，见 `pyb.freq()` ）。
     - ``sjw`` 是以时间量计的同步跳转宽度；数值可为1、2、3、4。
     - ``bs1`` 以时间量为单位定义了样本点的位置；该数值介于1-1024间。
     - ``bs2`` 以时间量为单位定义了传输点的位置；该数值介于1-16间。

   时间量（tq）是CAN总线的基本时间单位。Tq即CAN预分频器值除以PCLK1（内部外围总线1的频率）；确定PCLK1，详见 `pyb.freq()` 。

   一个位是由同步段组成的，通常即为1时间量。然后是位段1，位段2。样本点在位段1结束后。传输点在位段2结束后。波特率是二进时间，位时间即1 + BS1 + BS2与tq的乘积。

   例：当PCLK1=42MHz，prescaler=100，sjw=1，bs1=6，bs2=8，tq值为2.38微秒。位时间为35.7微秒，波特率为28kHz。

   .. only:: port_pyboard

      See page 680 of the STM32F405 datasheet for more details.

.. method:: CAN.deinit()

   关闭CAN总线。

.. method:: CAN.setfilter(bank, mode, fifo, params, \*, rtr)

   配置一个滤波器组：

   - ``bank`` 是将被配置的滤波器组。
   - ``mode`` 是滤波器运行的模式。
   - ``fifo`` 是fifo (0 or 1)信息储存的位置，前提是滤波器接受该信息。
   - ``params`` 是一组定义滤波器的值。该组值取决于mode参数。

   +-----------+---------------------------------------------------------+
   |``mode``   |参数数组内容                             |
   +===========+=========================================================+
   |CAN.LIST16 |将接受的4个16位id                   |
   +-----------+---------------------------------------------------------+
   |CAN.LIST32 |将接受的2个32位id                     |
   +-----------+---------------------------------------------------------+
   |CAN.MASK16 |2个16位id/掩码对。 例：(1, 3, 4, 4)              |
   |           | | 第一对，1和3将接受所有的bit 0 = 1和bit 1 = 0的id。         |
   |           | |                    |
   |           | | 第二对：4和4将接受所有bit 2 = 1的id。       |
   |           | |                                  |
   +-----------+---------------------------------------------------------+
   |CAN.MASK32 |像CAN.MASK16一样，但是只有一个32位id/掩码对。|
   +-----------+---------------------------------------------------------+

   - ``rtr`` 是一组布尔数组，该数组规定过滤器是否应接受远程传输请求消息。若该参数未给出，则默认所有条目为False。这一数组的长度取决于 `mode` 参数。

   +-----------+----------------------+
   |``mode``   |Rtr数组的长度   |
   +===========+======================+
   |CAN.LIST16 |4                     |
   +-----------+----------------------+
   |CAN.LIST32 |2                     |
   +-----------+----------------------+
   |CAN.MASK16 |2                     |
   +-----------+----------------------+
   |CAN.MASK32 |1                     |
   +-----------+----------------------+

.. method:: CAN.clearfilter(bank)

   清空并禁用一个滤波器组:

   - ``bank`` 是将被清空的滤波器组。

.. method:: CAN.any(fifo)

   若有消息在FIFO上等待，则返回 ``True`` ；若无，则返回 ``False`` 。

.. method:: CAN.recv(fifo, \*, timeout=5000)

   在总线上接收数据：

     - ``fifo`` 是一个整数，FIFO即在此接收。
     - ``timeout`` 是以毫秒计的等待接收的超时时长。

   返回值：包含4个值的元组

     - 消息的id。
     - 一个表明消息是否是RTR消息的布尔值。
     - FMI值（过滤器匹配指数）。
     - 一个包含数据的数组。.


.. method:: CAN.send(data, id, \*, timeout=0, rtr=False)

   在总线上发送消息：

     - ``data`` 是发送的数据（发送一个整数或一个缓冲区对象）。
     - ``id`` 是所发送的消息的id。
     - ``timeout`` 是以毫秒计的等待发送的超时时长。
     - ``rtr`` 是一个指定消息是否应该作为远程传输请求发送的布尔值。若 ``rtr`` 为True  ，则只使用 ``data`` 长度来填充框架的DLC插槽，
     而不使用 ``data`` 中的实际字节。

     若暂停时间为0，消息则置于三个硬件缓冲区中的其中一个，该方法立即返回。若三个缓冲区都被占用，则会引发异常。若暂停时间不为0，该方法会等待消息传输完毕。若该消息不能在指定时间内传输，则会引发异常。

   返回值： ``None``.

.. method:: CAN.rxcallback(fifo, fun)

   当空FIFO接收消息时，注册一个回调函数：

   - ``fifo`` 是接收的FIFO。
   - ``fun`` 是FIFO变为非空时要调用的函数。

   回调函数有两个参数：一个是CAN对象本身，一个是表明回调原因的整数。

   +--------+------------------------------------------------+
   | 原因 |                                                |
   +========+================================================+
   | 0      | 空FIFO接收消息。 |
   +--------+------------------------------------------------+
   | 1      | FIFO已满。                             |
   +--------+------------------------------------------------+
   | 2      | 由于FIFO已满，消息丢失。    |
   +--------+------------------------------------------------+

   回调用法示例::

     def cb0(bus, reason):
       print('cb0')
       if reason == 0:
           print('pending')
       if reason == 1:
           print('full')
       if reason == 2:
           print('overflow')

     can = CAN(1, CAN.LOOPBACK)
     can.rxcallback(0, cb0)

常量
---------

.. data:: CAN.NORMAL
.. data:: CAN.LOOPBACK
.. data:: CAN.SILENT
.. data:: CAN.SILENT_LOOPBACK

   CAN总线的模式

.. data:: CAN.LIST16
.. data:: CAN.MASK16
.. data:: CAN.LIST32
.. data:: CAN.MASK32

   过滤器运行模式
