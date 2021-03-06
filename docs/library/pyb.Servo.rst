.. currentmodule:: pyb
.. _pyb.Servo:

Servo类 –三线hobby舵机驱动
========================================

.. only:: port_pyboard

    伺服对象使用三条线（地线、电源线、信号线）控制标准hobby伺服电机。Pyboard上有4处电机可插入的位置：引脚X1到X4为信号引脚，其旁为4组电源和接地引脚。

    用法示例::

        import pyb

        s1 = pyb.Servo(1)   # create a servo object on position X1 在X1位置创建一个伺服对象
        s2 = pyb.Servo(2)   # create a servo object on position X2 在X2位置创建一个伺服对象

        s1.angle(45)        # move servo 1 to 45 degrees 将伺服1移动到45度
        s2.angle(0)         # move servo 2 to 0 degrees 将伺服2移动到0度

        # move servo1 and servo2 synchronously, taking 1500ms 同时移动伺服1和伺服2，耗时1500ms
        s1.angle(-60, 1500)
        s2.angle(30, 1500)

    .. note:: 伺服对象使用定时器(5)来生成PWM输出。您可将定时器(5)用于伺服控制或您的个人用途，但二者不能同时进行。

.. only:: port_moxingstm32f4

    Servo对象控制三线（地线，电源，信号）的标准hobby伺服电机。 (ground, power,signal)。墨星stm32有4个舵机引脚，分别是P13,P14,P15,P19。

    用法示例::

        import pyb

        s1 = pyb.Servo(1)   # create a servo object on position P13 在P13位置创建一个伺服对象
        s2 = pyb.Servo(2)   # create a servo object on position P14 在P14位置创建一个伺服对象

        s1.angle(45)        # move servo 1 to 45 degrees 将伺服1移动到45度
        s2.angle(0)         # move servo 2 to 0 degrees 将伺服2移动到0度

        # move servo1 and servo2 synchronously, taking 1500ms 同时移动伺服1和伺服2，耗时1500ms
        s1.angle(-60, 1500)
        s2.angle(30, 1500)

    .. note:: 伺服对象使用定时器(5)来生成PWM输出。您可将定时器(5)用于伺服控制或您的个人用途，但二者不能同时进行。

.. only:: port_openmvcam

    Servo对象控制三线（地线，电源，信号）的标准hobby伺服电机。 (ground, power, signal).

    用法示例::

        import pyb

        s1 = pyb.Servo(1)   # create a servo object on position P7
        s2 = pyb.Servo(2)   # create a servo object on position P8

        s1.angle(45)        # move servo 1 to 45 degrees
        s2.angle(0)         # move servo 2 to 0 degrees

        # move servo1 and servo2 synchronously, taking 1500ms
        s1.angle(-60, 1500)
        s2.angle(30, 1500)

    .. note:: Servo对象使用 Timer(5) 产生PWM输出。您可以使用 Timer(5) 进行伺服控制，也可以自己使用，但不能同时使用。

构造函数
------------

.. only:: port_pyboard

    .. class:: pyb.Servo(id)

       创建一个伺服对象。 ``id`` 为1-4，与引脚X1至X4相对应。

.. only:: port_pyboard

    .. class:: pyb.Servo(id)

       创建一个伺服对象。 ``id`` 为1-4，与引脚P13,P14,P15,P19相对应。

.. only:: port_pyboard

    .. class:: pyb.Servo(id)

       创建一个伺服对象。 ``id`` 为1-3，与引脚P7至P9相对应。


方法
-------

.. method:: Servo.angle([angle, time=0])

   若未给定参数，该函数返回当前角度。

   若给定函数，该函数设置servo的角度：

     - ``angle`` 是度数计的移动的角度。
     - ``time`` 是达到指定角度所用的毫秒数。若省略，则servo会尽快移动到新的位置。

.. method:: Servo.speed([speed, time=0])

   若未给定参数，该函数会返回当前速度。

   若给定参数，该函数设置servo的速度：

     - ``speed`` 是改变的速度，取值100-100。
     - ``time`` 是达到指定角度所用的毫秒数。若省略，则servo会尽快加速。

.. method:: Servo.pulse_width([value])

   若未给定参数，该函数会返回当前的原始脉宽值。

   若给定参数，该函数设置原始脉宽值。


.. method:: Servo.calibration([pulse_min, pulse_max, pulse_centre, [pulse_angle_90, pulse_speed_100]])

   若未给定参数，这个函数返回当前的5元组校准数据。

   若给定参数，该函数设定计时校准：

     - ``pulse_min`` 是允许的最小脉宽。
     - ``pulse_max`` 是允许的最大脉冲。
     - ``pulse_centre`` 是中心/零位置对应的脉宽。
     - ``pulse_angle_90`` 是90度对应的脉宽。
     - ``pulse_speed_100`` 是速度100对应的脉宽。
