计算器
------------------
模块介绍
^^^^^^^^^^^^^^^^^^^^^
此实验使用Solder_Key_1C模块和EDU-TFT-1.8模块

*实物图：

Solder_Key_1C模块

.. image:: ../picture/key1.jpg
   :width: 300px
   :height: 400px

EDU-TFT-1.8模块

.. image:: ../picture/lcd.jpg
   :width: 300px
   :height: 200px

思路说明
^^^^^^^^^
设计简单计算器，即不包括运算优先级与括号，仅能进行加减乘除运算，但是可以连续计算。
设计思路如下：
获得第一个数时，不做处理
获得第两个数后，计算出结果，计算出结果后情况和仅有一个数相同
当有第三个数进入时，和第二个数处理相同（因为进入时内存中只有一个数）

所以本次实验主要分为两部分，获取数据和计算数据


编程学习
^^^^^^^^^
本代码共需要导入如下库：

 :: 
 
  from pyb import Pin,Timer
  import lcd_show
  from lcd_show import *
  import pyb
  from pyb import Pin


完整代码如下：

 :: 

  from pyb import Pin,Timer
  import lcd_show
  from lcd_show import *
  import pyb
  from pyb import Pin

  #LCD
  usrspi = USR_SPI(scl=Pin('X6',Pin.OUT_PP), sda=Pin('X7', Pin.OUT),dc=Pin('X8', Pin.OUT))
  disp = DISPLAY(usrspi,cs=Pin('X5', Pin.OUT),res=Pin('X4', Pin.OUT),led_en=Pin('X3', Pin.OUT))
  x1 = Pin('X3',Pin.OUT_PP)
  R=[Pin('X9',Pin.OUT_PP),Pin('X10',Pin.OUT_PP),Pin('Y3',Pin.OUT_PP),Pin('Y4',Pin.OUT_PP)]
  C=[Pin('Y5',Pin.IN,Pin.PULL_UP),Pin('Y6',Pin.IN,Pin.PULL_UP),Pin('Y8',Pin.IN,Pin.PULL_UP),Pin('Y7',Pin.IN,Pin.PULL_UP)]
  formu=["0"]
  fir=0
  sec=0
  #将获取的string转化为float
  def transfer(number):
    value1=0
    value2=0
    tag=0
    for i in range(len(number)):
      if number[i]==".":#有小数
        tag=1
        for j in range(i):
          value1+=float(number[j])*pow(10,i-1-j)
        for j in range(i+1,len(number)):
          value1+=float(number[j])*pow(10,i-j)
    if tag==1:
      return value1
    for k in range(len(number)):
      value2+=float(number[k])*pow(10,len(number)-1-k)
    return value2
    

  remove=0
  disp.clr(disp.WHITE)
  key=1
  mark=0#加减乘除
  place=0#当前字符所在位置
  operator=0#运算符位置
  while True:#扫描键盘
    for i in range(0,4):
      R[i].low()
      for k in range(0,4):
        if k!=i:
          R[k].high()
      for j in range(0,4):
        if i==0 and j==0 and C[j].value()==0:
          pyb.delay(10)
          #if C[j].value()==0:
          print('7')
          formu+="7"
          place=place+1
        elif i==0 and j==1 and C[j].value()==0:
          pyb.delay(10)
          #if C[j].value()==0:
          print('8')
          formu+="8"
          place=place+1
        elif i==0 and j==2 and C[j].value()==0:
          pyb.delay(10)
          #if C[j].value()==0:
          print('9')
          formu+="9"
          place=place+1
        elif i==0 and j==3 and C[j].value()==0:
          pyb.delay(10)
          #if C[j].value()==0:
          print('/')
          formu+="/"
          place=place+1
        elif i==1 and j==0 and C[j].value()==0:
          pyb.delay(10)
          #if C[j].value()==0:
          print('4')
          formu+="4"
          place=place+1
        elif i==1 and j==1 and C[j].value()==0:
          pyb.delay(10)
          #if C[j].value()==0:
          print('5')
          formu+="5"
          place=place+1
        elif i==1 and j==2 and C[j].value()==0:
          pyb.delay(10)
          #if C[j].value()==0:
          print('6')
          formu+="6"
          place=place+1
        elif i==1 and j==3 and C[j].value()==0:
          pyb.delay(10)
          #if C[j].value()==0:
          print('*')
          formu+="*"
          place=place+1
        elif i==2 and j==0 and C[j].value()==0:
          pyb.delay(10)
          #if C[j].value()==0:
          print('1')
          formu+="1"
          place=place+1
        elif i==2 and j==1 and C[j].value()==0:
          pyb.delay(10)
          #if C[j].value()==0:
          print('2')
          formu+="2"
          place=place+1
        elif i==2 and j==2 and C[j].value()==0:
          pyb.delay(10)
          #if C[j].value()==0:
          print('3')
          formu+="3"
          place=place+1
        elif i==2 and j==3 and C[j].value()==0:
          pyb.delay(10)
          #if C[j].value()==0:
          print('-')
          formu+="-"
          place=place+1
        elif i==3 and j==0 and C[j].value()==0:
          pyb.delay(10)
          #if C[j].value()==0:
          print('0')
          formu+="0"
          place=place+1
        elif i==3 and j==1 and C[j].value()==0:
          pyb.delay(10)
          #if C[j].value()==0:
          print('.')
          formu+="."
          place=place+1
        elif i==3 and j==2 and C[j].value()==0:
          pyb.delay(10)
          #if C[j].value()==0:
          print('=')
          formu+="="
          place=place+1
        elif i==3 and j==3 and C[j].value()==0:
          pyb.delay(10)
          #if C[j].value()==0:
          print('+')
          formu+="+"
          place=place+1
      pyb.delay(50)
    if len(formu)>1:
      if (((formu[0]=="+") or (formu[0]=="-") or (formu[0]=="*") or (formu[0]=="/")) and len(formu)>1):
        formu=formu[1:]
      if (((formu[-1]=="+") or (formu[-1]=="-") or (formu[-1]=="*") or (formu[-1]=="/") or (formu[-1]=="=")) and len(formu)>=1):
        operator=place
        if remove==0:
          formu=formu[1:]
          remove=remove+1
        if key ==1:
          fir=transfer(formu[0:operator-1])#翻译第一个数
          print("fir  is : ",end=" ")
          print(fir)
          disp.putstr(6,5,str(fir),0x0000)
          if formu[operator-1]=="+":
            mark=0
            disp.putstr(4,6,"+",0x0000)
          if formu[operator-1]=="-":
            mark=1
            disp.putstr(4,6,"-",0x0000)
          if formu[operator-1]=="*":
            mark=2
            disp.putstr(4,6,"*",0x0000)
          if formu[operator-1]=="/":
            mark=3
            disp.putstr(4,6,"/",0x0000)
          formu=list(formu[-1])
          key=key+1#找到第一个数
        else :#不是第一个数
          sec=transfer(formu[:-1])
          print("sec  is : ",end=" ")
          print(sec)
          disp.putstr(6,6,str(sec),0x0000)
          if mark==0:
            fir=fir+sec
          if mark ==1:
            fir=fir-sec
          if mark==2:
            fir=fir*sec
          if mark==3:
            fir=fir/sec
          if len(formu)>=1:#继续计算
            #清空现有的数据显示，添加符号
            if formu[-1]=="+":
              disp.putrect(36,54,8*len(str(sec)),8,0xffff)
              disp.putrect(36,45,8*len(str(fir)),8,0xffff)
              disp.putrect(24,54,8,8,0xffff)
              mark=0
              disp.putstr(4,6,"+",0x0000)
            if formu[-1]=="-":
              disp.putrect(36,54,8*len(str(sec)),8,0xffff)
              disp.putrect(36,45,8*len(str(fir)),8,0xffff)
              disp.putrect(24,54,8,8,0xffff)
              mark=1
              disp.putstr(4,6,"-",0x0000)
            if formu[-1]=="*":
              disp.putrect(36,54,8*len(str(sec)),8,0xffff)
              disp.putrect(36,45,8*len(str(fir)),8,0xffff)
              disp.putrect(24,54,8,8,0xffff)
              mark=2
              disp.putstr(4,6,"*",0x0000)
            if formu[-1]=="/":
              disp.putrect(36,54,8*len(str(sec)),8,0xffff)
              disp.putrect(36,45,8*len(str(fir)),8,0xffff)
              disp.putrect(24,54,8,8,0xffff)
              mark=3
              disp.putstr(4,6,"/",0x0000)
            formu=formu[-1]
            #在fir位置显示结果
            disp.putstr(6,5,str(fir),0x0000)
    if (len(formu)>=1 and formu[-1]=="="):
      #print(fir)
      disp.putstr(6,7,str(fir),0x0000)

输入算式：42-6+1.6-2=

效果如下:

.. image:: ../picture/computer.jpg
   :width: 300px
   :height: 300px

