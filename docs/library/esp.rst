:mod:`esp` --- 与ESP32相关的函数
===============================================

.. module:: esp
    :synopsis: functions related to the ESP8266

The ``esp`` module 模块包含与模块相关的特定函数。


函数
---------

.. function:: sleep_type([sleep_type])

    获取或设置睡眠类型。

    若给定 *sleep_type* 参数，则将睡眠类型设置为其值。若调用函数未给定参数，则返回当前的睡眠类型。

    可用的睡眠类型被定义为常量：

        * ``SLEEP_NONE`` -- 所有函数启用
        * ``SLEEP_MODEM`` -- 调制解调器睡眠，关闭WiFi调制解调器电路
        * ``SLEEP_LIGHT`` -- 轻度睡眠，关闭WiFi调制解调器电路，并定期暂停处理器。

    条件允许时，系统会自动进入设定的睡眠模式。

.. function:: deepsleep(time=0)

    进入深度睡眠。

    除RTC时钟电路外的整个模块断电。指定时间后，若引脚16与重置引脚相连接，RTC时钟电路可用于重启模块。
    否则该模块将始终处于睡眠状态，直至手动重启。

.. function:: flash_id()

    读取闪存的设备ID。

.. function:: flash_read(byte_offset, length_or_buffer)

.. function:: flash_write(byte_offset, bytes)

.. function:: flash_erase(sector_no)

.. function:: set_native_code_location(start, length)

    设置本机代码放置后的位置以便执行
    编译。当``@ micropython.native``时会发出本机代码，
    应用``@ micropython.viper``和``@ micropython.asm_xtensa``装饰器
    一个功能。 ESP8266必须从iRAM或更低版本执行代码
    1MByte的flash（内存映射），此功能控制
    地点。

    如果* start *和* length *都是“None”，则本机代码位置为
    设置为iRAM1区域末尾的未使用存储器部分。该
    这个未使用部分的大小取决于固件，通常是相当的
    小（大约500字节），并足以存储一些非常小的
    功能。使用这个iRAM1区域的优点是它没有
    通过写信来磨损。

    如果* start *和* length *都不是``None``那么它们应该是整数。
    * start *应指定从闪存开始处的字节偏移量
    应存储哪些本机代码。 * length *指定的字节数
    flash from * start *可用于存储本机代码。 *开始*和*长度*
    应该是扇区大小的倍数（4096字节）。闪光灯会
    在写入之前自动擦除，所以一定要使用的区域
    没有使用的闪存，例如固件或
    文件系统。

    使用闪存存储本机代码时* start + length *必须更少
    大于或等于1MByte。请注意，如果重复闪光灯可能会磨损
    删除（和写入）是为了节省使用此功能。
    特别是，需要重新编译本机代码并将其重写为flash
    在每次启动时（包括从深睡眠唤醒）。

    在上述两种情况下，如果没有剩余空间，使用iRAM1或闪光灯
    在指定的区域然后在函数上使用本机装饰器
    将导致在编译期间引发的“MemoryError”异常
    那个功能。
