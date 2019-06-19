.. currentmodule:: pyb
.. _pyb.I2C:

I2C类 -- 二线串行协议
=======================================

I2C是一个设备间传输的二线串行协议。其物理层包括两条线：SCL和SDA，分别为时钟和数据线

I2C对象创建在特定总线上。可在创建时或创建后初始化

Example::

    from pyb import I2C

    i2c = I2C(1)                         # 创建总线1
    i2c = I2C(1, I2C.MASTER)             # 初始化为主模式
    i2c.init(I2C.MASTER, baudrate=20000) # 初始化为主模式 波特率20000
    i2c.init(I2C.SLAVE, addr=0x42)       # 初始化为从模式地址0x42
    i2c.deinit()                         # 关闭

打印i2c对象可以提供关于其配置的信息。

基本方法是发送和接收:::

    i2c.send('abc')      # 发送 3 bytes
    i2c.send(0x42)       # 发送以数字形式给出的单个字节
    data = i2c.recv(3)   # 接收 3 bytes

要接收数据，先创建一个 bytearray::

    data = bytearray(3)  # 创建一个缓冲区
    i2c.recv(data)       # 接收3个字节，并将其写入数据

你可以指定一个超时时长（单位：毫秒）::

    i2c.send(b'123', timeout=2000)   # 超时2秒

主机必须指定接收方地址::

    i2c.init(I2C.MASTER)
    i2c.send('123', 0x42)        # 使用0x42地址向子机发送3个字节
    i2c.send(b'456', addr=0x42)  

主机也有其他方法::

    i2c.is_ready(0x42)           # 检查从机0x42是否准备充分
    i2c.scan()                   # 在总线上浏览从机，返回一个有效地址列表
    i2c.mem_read(3, 0x42, 2)     # 从从机0x42的内存中读取3个字节，从从机的地址2开始
    i2c.mem_write('abc', 0x42, 2, timeout=1000) # 将“abc”写入从机内存
                                                # 从从机的地址2开始，1秒后超时

构造函数
------------

.. class:: pyb.I2C(bus, ...)

   在给定总线上构建一个I2C对象。 ``bus`` 可为1,2。在没有其他参数的情况下，可创建I2C对象，但未进行初始化
   该对象的设置来自总线的最后一次初始化，若存在的话）。若给定额外参数，则总线进行初始化。初始化参数详见 ``init``。

   在Pyboards V1.0 和 V1.1 中I2C总线的物理引脚为:

     - ``I2C(1)`` 在X引脚位置: ``(SCL, SDA) = (X9, X10) = (PB6, PB7)``
     - ``I2C(2)`` 在Y引脚位置: ``(SCL, SDA) = (Y9, Y10) = (PB10, PB11)``

   在 Pyboard Lite 中:

     - ``I2C(1)`` 在X引脚位置: ``(SCL, SDA) = (X9, X10) = (PB6, PB7)``
     - ``I2C(3)`` 在Y引脚位置: ``(SCL, SDA) = (Y9, Y10) = (PA8, PB8)``

方法
-------

.. method:: I2C.deinit()

   关闭I2C总线.

.. method:: I2C.init(mode, \*, addr=0x12, baudrate=400000, gencall=False, dma=False)

  使用给定参数初始化I2C总线:

     - ``mode`` 必须为 ``I2C.MASTER`` 或 ``I2C.SLAVE``
     - ``addr`` 是一个7位地址(只对从机有效)
     - ``baudrate`` 是SCL的时钟频率(只对主机有效)
     - ``gencall`` 是指是否支持一般调用模式
     - ``dma`` 是否允许将DMA用于I2C传输（请注意，DMA传输具有更精确的时序，但目前不能正确处理总线错误）

.. method:: I2C.is_ready(addr)

   检查I2C设备是否响应给定地址。只在主模式有效.

.. method:: I2C.mem_read(data, addr, memaddr, \*, timeout=5000, addr_size=8)

   从I2C设备内存中读取:

     - ``data`` 可为一个要读入的整数(要读取的字节数量)或缓冲区
     - ``addr`` 是I2C设备地址
     - ``memaddr`` 是I2C设备中内存的位置
     - ``timeout`` 是以毫秒计的等待读取的超时时长
     - ``addr_size`` 选择memaddr宽度：8或16位

   返回读取数据。只在主模式下有效。

.. method:: I2C.mem_write(data, addr, memaddr, \*, timeout=5000, addr_size=8)

   写入I2C设备的内存:

     - ``data`` 可为一个用来写入的整数或缓冲区
     - ``addr`` 是I2C设备的地址。
     - ``memaddr`` 是I2C设备中内存的位置
     - ``timeout`` 是以毫秒计的等待写入的超时时长
     - ``addr_size`` 选择memaddr宽度：8或16位

   返回 ``None``。
   只在主模式下有效。

.. method:: I2C.recv(recv, addr=0x00, \*, timeout=5000)

   在总线上接收数据:

     - ``recv`` 可为一个整数，即接收的字节的数量；或为一个使用收到的字节来填充的可变缓冲区。
     - ``addr`` 是用以接收的地址（只在主模式下需要）
     - ``timeout`` 是以毫秒计的等待接收的超时时长

   返回值：若 ``recv`` 为一个整数，则有一个新的字节缓冲区来接收，否则这一缓冲区将传递给``recv``

.. method:: I2C.send(send, addr=0x00, \*, timeout=5000)

   在总线上发送数据:

     - ``send`` 是要发送的数据（一个整数或一个缓冲区对象）
     - ``addr`` 是要发送到的地址（只在主模式下需要）
     - ``timeout`` 是以毫秒计的等待发送的超时时长

   返回值: ``None``。

.. method:: I2C.scan()

   浏览从0x01到0x7f的所有I2C地址，并返回一个响应的地址的列表。只在主模式下有效。

常量
---------

.. data:: I2C.MASTER

   将总线初始化为主模式

.. data:: I2C.SLAVE

   将总线初始化为从模式