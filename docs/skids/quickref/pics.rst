.. _pics:

互发表情设计与实现
============================

设计思路
----------------------------

- 连接到mqtt服务器
- 时刻检查是否收到消息，监听按键动作，选择表情并判断是否发送表情
- 接收或发送消息时在屏幕上显示出来


代码实现
----------------------------
首先导入需要的库

::

	import ubitmap
	import screen
	import time
	import text
	import network
	from machine import UART,Pin
	import utime
	from umqtt.simple import MQTTClient
	
初始化pics类,配制相关参数mqtt服务器地址，端口等
::

	class pics():
		def __init__(self):
			self.keys = [Pin(p, Pin.IN) for p in [35, 36, 39, 34]]
			self.keymatch = ["Key1", "Key2", "Key3", "Key4"]
			self.select=1
			self.displayInit()
			self.wifi_name = "NEUI"
			self.wifi_SSID = "NEUI3187"
			#MQTT服务端信息
			self.SERVER = "112.125.89.85"   #MQTT服务器地址
			self.SERVER_PORT = 3881         #MQTT服务器端口
			self.DEVICE_ID = "wc001"        #设备ID
			self.TOPIC1 = b"/cloud-skids/online/dev/" + self.DEVICE_ID
			self.TOPIC2 = b"/cloud-skids/message/server/" + self.DEVICE_ID
			self.CLIENT_ID = "f25410646a8348f8a1726a3890ad8f73"
			self.uart = UART(1, baudrate=115200, bits=8, parity=0, rx=18, tx=19, stop=1)
			#设备状态
			self.ON = "1"
			self.OFF = "0"
			self.d=" "#初始化要发送的信息
			self.c = MQTTClient(self.CLIENT_ID, self.SERVER, self.SERVER_PORT)#定义一个mqtt实例

画程序界面
::

		def drawInterface(self):#界面初始化
			bmp1=ubitmap.BitmapFromFile("pic/boy")
			bmp2=ubitmap.BitmapFromFile("pic/girl")
			bmp1.draw(20,200)#显示boy图片
			bmp2.draw(140,200)#显示girl图片
			screen.drawline(0, 160, 240, 160, 2, 0xff0000)

连接wifi接口，使用STA模式
::
			
		def do_connect(self):
			sta_if = network.WLAN(network.STA_IF)    #STA模式
			ap_if = network.WLAN(network.AP_IF)      #AP模式
			if ap_if.active():
				ap_if.active(False)                  #关闭AP
			if not sta_if.isconnected():
				print('Connecting to network...')
			sta_if.active(True)                      #激活STA
			sta_if.connect(self.wifi_name, self.wifi_SSID)     #WiFi的SSID和密码
			while not sta_if.isconnected():
				pass
			print('Network config:', sta_if.ifconfig())
			gc.collect()

画默认表情焦点
::

		def selectInit(self):#选择表情初始化
			screen.drawline(20, 200, 92, 200, 2, 0xff0000)
			screen.drawline(92, 200, 92, 272, 2, 0xff0000)
			screen.drawline(92, 272, 20, 272, 2, 0xff0000)
			screen.drawline(20, 272, 20, 200, 2, 0xff0000)

程序界面初始化接口
::

		def displayInit(self):#初始化
			screen.clear()
			self.drawInterface()
			self.selectInit()
			
初始化mqtt服务器连接
::

		def esp(self):
			self.c.set_callback(self.sub_cb)    #设置回调
			self.c.connect()
			print("连接到服务器：%s" % self.SERVER)
			self.c.publish(self.TOPIC1, self.ON)     #发布“1”到TOPIC1
			self.c.subscribe(self.TOPIC2)       #订阅TOPIC
			#display.text("从微信取得信息", 20, 20, 0xf000, 0xffff)
		
键盘监听接口
::
		
		def keyboardEvent(self, key):
			if self.keymatch[key] == "Key1":#右移键，选择要发送的表情
				if self.select%2==1:#用红色框选中boy表情
					screen.drawline(20, 200, 92, 200, 2, 0xffffff)
					screen.drawline(92, 200, 92, 272, 2, 0xffffff)
					screen.drawline(92, 272, 20, 272, 2, 0xffffff)
					screen.drawline(20, 272, 20, 200, 2, 0xffffff)
					screen.drawline(140, 200, 212, 200, 2, 0xff0000)
					screen.drawline(212, 200, 212, 272, 2, 0xff0000)
					screen.drawline(212, 272, 140, 272, 2, 0xff0000)
					screen.drawline(140, 272, 140, 200, 2, 0xff0000)
					self.select+=1
				else:#用红色框选中girl表情
					screen.drawline(140, 200, 212, 200, 2, 0xffffff)
					screen.drawline(212, 200, 212, 272, 2, 0xffffff)
					screen.drawline(212, 272, 140, 272, 2, 0xffffff)
					screen.drawline(140, 272, 140, 200, 2, 0xffffff)
					screen.drawline(20, 200, 92, 200, 2, 0xff0000)
					screen.drawline(92, 200, 92, 272, 2, 0xff0000)
					screen.drawline(92, 272, 20, 272, 2, 0xff0000)
					screen.drawline(20, 272, 20, 200, 2, 0xff0000)
					self.select+=1
			if self.keymatch[key] == "Key3":#发送表情按键
				if self.select%2==1:#显示已发送boy表情
					bmp1=ubitmap.BitmapFromFile("pic/boy")
					bmp1.draw(140,40)
					self.d="001"
					self.c.publish(self.TOPIC2,self.d)#给服务器发送boy表情的号码
				else:#显示已发送girl表情
					bmp2=ubitmap.BitmapFromFile("pic/girl")
					bmp2.draw(140,40)
					self.d="002"
					self.c.publish(self.TOPIC2,self.d)#给服务器发送girl表情的号码

服务器回调接口
::

		def sub_cb(self,topic, message):#从服务器接受信息
			message = message.decode()
			print("服务器发来信息：%s" % message)
			#global count
			if message=="001":#收到boy表情号码显示boy表情
				bmp1=ubitmap.BitmapFromFile("pic/boy")
				bmp1.draw(140,40)
			elif message=="002":#收到girl表情号码显示girl表情
				bmp1=ubitmap.BitmapFromFile("pic/girl")
				bmp1.draw(140,40)
			
程序启动接口
::						
					
		def start(self):
			try:
				while True:
					self.c.check_msg()#检查是否收到信息
					i = 0#用来辅助判断那个按键被按下
					j = -1
					for k in self.keys:#检查按键是否被按下
						if (k.value() == 0):##如果按键被按下
							if i != j:
								j = i
								self.keyboardEvent(i)#触发相应按键对应事件
						i = i + 1
						if (i > 3):
							i = 0
					time.sleep_ms(130)
			finally:
					self.c.disconnect()
					print("MQTT连接断开")

程序主函数
::
				
	if __name__ == '__main__':
		p = pics()
		p.do_connect()
		p.esp()
		p.start()
				

效果展示
----------------------------


.. image:: img/pics.png


