.. _introduction:

简单的传感网络介绍
=======================

架构图
-----------------------

.. image:: img/architecture1.jpg

连接图
-----------------------

.. image:: img/connect1.jpg

程序说明
-----------------------

TBSource(Sensor Controller):
  + sensor.py：传感器控制库，包括DHT11、BH1750、SPO2类
  + TB_Sensor_EndDevice.py：主程序，获取传感数据，封装成AT指令，通过UART发给ZigBee模块

SkidsSource(Gateway):
  + umqtt/simple.py：MQTT函数库
  + ATRT.py：AT指令收发库，用于对ZigBee协调器UART发来的AT指令解析、封装数据成AT指令
  + MSGP.py：MQTT消息处理库，用于对MQTT主题收到的消息进行解析或封装
  + LCD.py：LCD显示库，用于驱动LCD显示收到的传感数据
  + SK_Gateway.py：主程序，连接WiFi，创建一个线程监听串口传感数据，主线程监听主题MQTT消息

使用说明
-----------------------

  + 将TBSource(Sensor Controller)拷贝保存到连接着传感器模块的TB板的PYBFLASH中，主程序名改为main.py，然后复位启动。
  + 将SkidsSource(Gateway)按以上目录结构用uPyCraft工具下载到网关Skids（ZigBee烧的是协调器程序）中，然后运行主程序。
  + 之后网关Skids在LCD上显示传感数据信息，并发布到MQTT主题。
