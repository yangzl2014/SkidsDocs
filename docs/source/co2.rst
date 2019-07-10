二氧化碳传感器
------------------
模块介绍
^^^^^^^^^^^^^^^^^^^^^
MH-Z14A二氧化碳气体传感器（以下简称传感器）是一个通用智能小型传感器，
利用非色散红外（NDIR）原理对空气中存在的CO2进行探测，具有很好的选择性和无氧气依赖性，寿命长。
内置温度补偿；同时具有数字输出、模拟输出及PWM输出，方便使用。
该传感器是将成熟的红外吸收气体检测技术与精密光路设计、精良电路设计紧密结合而制作出的高性能传感器

*实物图：

MH-Z14A二氧化碳模块

.. image:: ../picture/co2.jpg
   :width: 300px
   :height: 200px

接线
^^^^^^^^^
二氧化碳模块T连接TB板X2（UART4_RX),
二氧化碳模块R连接TB板X1（UART4_TX),
二氧化碳模块V+连接TB板3V3,
二氧化碳模块V-连接TB板GND

原理学习
^^^^^^^^^
本实验使用串口输出（UART）模式，将串口波特率设置为9600，数据位设置为8位，停止位设置为1位，奇偶校验设置为无。
读气体浓度值命令[0xff, 0x01, 0x86, 0x00, 0x00, 0x00, 0x00, 0x00, 0x79].
返回值[0xff, 0x86, HIGH,LOW , - , - , - , - ,校验和]。
气体浓度=HIGH*256+LOW。
校准传感器零点命令[0xff, 0x01, 0x87, 0x00, 0x00, 0x00, 0x00, 0x00, 0x78]。
校验和=（取反（Byte1+Byte2+Byte3+Byte4+Byte5+Byte6+Byte7））+1。

编程学习
^^^^^^^^^
打开main.py文件开始编写代码:

导入头文件:

 :: 

	import pyb
	import time
	import json
	from pyb import Pin
	from pyb import UART
	class MHZ14A():
		PACKET = [0xff, 0x01, 0x86, 0x00, 0x00, 0x00, 0x00, 0x00, 0x79]#读气体浓度值命令
		ZERO = [0xff, 0x01, 0x87, 0x00, 0x00, 0x00, 0x00, 0x00, 0x78]#校准传感器零点命令
		def __init__(self):#初始化
			self.uart = UART(4, 9600,bits=8,parity = None)                        
			time.sleep(2)
		def zero(self):#校准零点
			#print(bytearray(MHZ14A.ZERO))
			self.uart.write(bytearray(MHZ14A.ZERO))
		
		def get(self):#读取气体浓度
			self.uart.write(bytearray(MHZ14A.PACKET))
			#print(bytearray(MHZ14A.PACKET))
			res=self.uart.read(9)
			#print(res)
			#print(len(res))
			#for i in range(0,len(res))
			res = bytearray(res)
			checksum = 0xff & (~(res[1] + res[2] + res[3] + res[4] + res[5] + res[6] + res[7]) + 1)
			if res[8] == checksum:#校验
				print('ppm:',((res[2] << 8) | res[3]))
				#print(res[4])
				return {
					"ppm": (res[2] << 8) | res[3]
				}
			else:
				print ("checksum: " + hex(checksum))
				return -1
		def deinit(self):
			self.uart.deinit()


	if __name__ == '__main__':
		A=MHZ14A()
		A.zero()
		A.get()
		A.deinit()
		while(1):
			A=MHZ14A()
			#A.zero()
			A.get()
			A.deinit()




实验现象
^^^^^^^^^^^^^^^^^^^^^

加载程序。显示空气中二氧化碳浓度。

.. image:: ../picture/ppm.png
   :width: 300px
   :height: 400px
