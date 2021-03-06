.. currentmodule:: pyb
.. _pyb.RTC:

class RTC -- 实时时钟
============================

RTC 是一个独立的时钟，可追踪日期和时间。

用法示例::

    rtc = pyb.RTC()
    rtc.datetime((2014, 5, 1, 4, 13, 0, 0, 0))
    print(rtc.datetime())


构造函数
------------

.. class:: pyb.RTC()

   创建一个RTC对象。


方法
-------

.. method:: RTC.datetime([datetimetuple])

   获取或设置RTC的日期和时间。

   若无参数，则该方法返回一个当前日期和时间的8元组。若有一参数（8元组），则设置日期和时间。

   .. only:: port_pyboard or port_openmvcam or port_moxingstm32f4

       该8元组有以下形式：

           （年，月，日，一周某天，小时，分钟，秒钟，亚秒）

       ``weekday`` 从周一到周日对应1-7。

       ``subseconds`` 从255倒数到0。

.. only:: port_pyboard or port_openmvcam or port_moxingstm32f4

    .. method:: RTC.wakeup(timeout, callback=None)

       将RTC唤醒定时器设置为在每次 ``timeout`` 毫秒后重复触发。这一触发可将pyboard从两种睡眠状态中唤醒： `pyb.stop()` 和 `pyb.standby()` 。

       若 ``timeout`` 为 ``None`` ，则禁用唤醒定时器。

       若给定 ``callback`` ，则在唤醒定时器的每次触发时执行。 ``callback`` 须取一个参数。

    .. method:: RTC.info()

       获取关于启动时间和重置源的信息。

        - 低0xffff为RTC启动所使用的毫秒数。
        - 若发生电源复位，则设置位0x10000。
        - 若发生外部重置，则设置位0x20000。

    .. method:: RTC.calibration(cal)

       获取或设置RTC校准。

       若无参数， ``calibration()`` 则返回当前校准值，即一个介于[-511 : 512]的整数。若有一个参数，则设置RTC校准。

       RTC平滑校准机制通过在32秒（对应2^20时钟tick）内从32768赫兹的时钟频率中增减给定数量的tick来调整RTC的时钟频率。
       每增加一tick，会使时钟加速1 part in 2^20（或0.954 ppm）；同理，RTC时钟将会在负值下减速。
       可用的校准范围为(-511 * 0.954) ~= -487.5 ppm up to (512 * 0.954) ~= 488.5 ppm。

