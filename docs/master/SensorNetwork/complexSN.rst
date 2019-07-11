.. _introduction:

复杂的传感网络介绍
=======================

架构图
-----------------------

.. image:: img/architecture2.jpg

连接图
-----------------------

.. image:: img/connect2.jpg

程序说明
-----------------------

TBSource(Sensor Controller):
  + sensor.py：传感器控制库，包括DHT11、BH1750、SPO2类
  + TB_Sensor_EndDevice.py：主程序，设置PANID，获取传感数据，封装成AT指令，通过UART发给ZigBee模块

TBSource(Motor Controller):
  + control.py：设备控制库，包括Motor类
  + TB_Control_EndDevice.py：主程序，设置PANID，接收AT指令并解析，控制设备进行所要求的操作

SkidsSource(Gateway):
  + umqtt/simple.py：MQTT函数库
  + ATRT.py：AT指令收发库，用于对ZigBee协调器UART发来的AT指令解析，封装数据成AT指令
  + MSGP.py：MQTT消息处理库，用于对MQTT主题收到的消息进行解析或封装
  + LCD.py：LCD显示库，用于驱动LCD显示收到的传感数据
  + SK_Gateway_co.py：主程序，设置PANID，连接WiFi，接收传感数据并发布、群发，订阅主题接收MQTT消息并下发

SkidsSource(EndDevice):
  + LCD.py：LCD显示库，用于驱动LCD显示收到的传感数据
  + SK_EndDevice.py：主程序，设置PANID，接收ZigBee终端UART发来的AT指令，解析数据，驱动LCD显示

使用说明
-----------------------

1. 修改TB_Sensor_EndDevice.py、TB_Control_EndDevice.py、SK_Gateway_co.py、SK_EndDevice.py中的PANID，要唯一，以建立自己的ZigBee网，防止与其他人冲突。
#. 修改SK_Gateway_si.py中的WiFi名称和密码。
#. 将TBSource(Sensor Controller)拷贝到连接着传感器模块的TB板的PYBFLASH中，主程序名改为main.py，然后复位启动。
#. 将TBSource(Motor Controller)拷贝到连接着步进电机模块的TB板的PYBFLASH中，主程序名改为main.py，然后复位启动。
#. 将SkidsSource(Gateway)按以上目录结构用uPyCraft工具下载到网关Skids（ZigBee烧的是协调器固件）中，然后运行主程序。
#. 将SkidsSource(EndDevice)按目录结构用uPyCraft工具下载到终端Skids（ZigBee烧的是终端固件）中，然后运行主程序。
#. 网关Skids接收传感数据，转发到终端Skids在LCD上显示，并发布数据到MQTT主题，同时也可接收MQTT下发控制指令。

（注：如果要修改设置的PANID，需先重启skids，再执行主程序。）
