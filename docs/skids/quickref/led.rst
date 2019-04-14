.. _led:

LED与跑马灯
============================


LED简介
----------------------------

Skids集成了4个LED，在设备的顶部（屏幕上方），其颜色分别为红、黄、蓝、绿：

.. image:: img/led1.jpg
    :alt: led
    :width: 340px

- 控制引脚

  + LED控制使能引脚：PIN 2
  + LED1控制引脚：PIN 14（红色）
  + LED2控制引脚：PIN 32（黄色）
  + LED3控制引脚：PIN 33（蓝色）
  + LED4控制引脚：PIN 27（绿色）


控制代码
----------------------------

- LED控制方法
::

    # 导入用于引脚控制的Python库
    from machine import Pin

    # 获取引脚
    led_en = Pin(2, Pin.OUT)
    led1 = Pin(14, Pin.OUT)
    led2 = Pin(27, Pin.OUT)
    led3 = Pin(33, Pin.OUT)
    led4 = Pin(32, Pin.OUT)

    # 使能LED控制，设置后4个LED全亮
    led_en.value(1)

    # 关闭LED1
    led1.value(1)

    # 开启LED1
    led1.value(0)


- 跑马灯实现
::

    from machine import Pin
    import time

    #获取引脚
    led_en = Pin(2, Pin.OUT)
    led1 = Pin(14, Pin.OUT)
    led2 = Pin(27, Pin.OUT)
    led3 = Pin(33, Pin.OUT)
    led4 = Pin(32, Pin.OUT)
    #定义LED数组，以便于后续操作
    leds = [led1, led2, led3, led4]

    #使能LED控制
    led_en.value(1)

    #初始化循环变量
    i = 0

    #开始循环
    while True:
        #将所有LED关闭
        for l in leds:
            l.value(1)
        #开启特定的LED
        leds[i].value(0)
        #计算下一个需要开启的LED编号
        i = (i+1)%4
        #延时1秒
        time.sleep(1)
