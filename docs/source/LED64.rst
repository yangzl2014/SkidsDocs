LED点阵滚动显示文字
------------------

模块介绍
^^^^^^^^^^^^^^^^^^^^^
此实验使用Solder_DisCan_1B模块

*实物图：

Solder_DisCan_1B模块

.. image:: ../picture/led1.jpg
   :width: 300px
   :height: 200px
编程学习
^^^^^^^^^
打开main.py文件开始编写代码:
导入库文件:

 :: 

  from machine import Pin
  from micropython import const
  import time
  import ustruct
  import framebuf
  from pyb import Pin

之后开始初始化引脚，代码为：

 ::

  r=[Pin(i, Pin.OUT_PP) for i in ['X2','X1','X3','X4','X5','X6','X8','X7']]
  for i in range (8):
    r[i].high()

  c=[Pin(i, Pin.OUT_PP) for i in ['Y2','Y1','Y3','Y4','Y5','Y6','Y8','Y7']]
  for i in range (8):
    c[i].high()

.. Note:: 通过两个循环把行，列的所有引脚拉高，熄灭LED点阵，只有行列的引脚同时为低电平时灯才能亮。

设置完毕后，即可编写函数实现各种功能，代码如下：

全局变量初始化：
 ::

  png=[#第一次初始化
    "00000000",
    "",
    "",
    "",
    "",
    "",
    "",
    ""
  ]

  Letter=[#第一次初始化
      [[1,2,3,4,5,6,7,8],[[0,0,0,0,0,0,0,0,],[],[],[],[],[],[],[],]],
      [[1,2,3,4,5,6,7,8],[[0,0,0,0,0,0,0,0,],[],[],[],[],[],[],[],]],
      [[1,2,3,4,5,6,7,8],[[0,0,0,0,0,0,0,0,],[],[],[],[],[],[],[],]],
      [[1,2,3,4,5,6,7,8],[[0,0,0,0,0,0,0,0,],[],[],[],[],[],[],[],]],
      [[1,2,3,4,5,6,7,8],[[0,0,0,0,0,0,0,0,],[],[],[],[],[],[],[],]],
      [[1,2,3,4,5,6,7,8],[[0,0,0,0,0,0,0,0,],[],[],[],[],[],[],[],]],
      [[1,2,3,4,5,6,7,8],[[0,0,0,0,0,0,0,0,],[],[],[],[],[],[],[],]],
      [[1,2,3,4,5,6,7,8],[[0,0,0,0,0,0,0,0,],[],[],[],[],[],[],[],]]
      ]

代码实现：
 ::

  def line(row,colDisp):#负责一行灯的亮与灭，row行号，colDisp为列号数组
    r[row-1].low() #打开某行
    for j in colDisp: 
      if (j>=0 and j<=7):
        c[j].low()#开启
        pyb.delay(1)
        c[j].high()#熄灭
    r[row-1].high()#关闭行


  def reser_Letter():#重置Letter
    global Letter
    global l
    l=0
    Letter=[
      [[1,2,3,4,5,6,7,8],[[0,0,0,0,0,0,0,0,],[],[],[],[],[],[],[],]],
      [[1,2,3,4,5,6,7,8],[[0,0,0,0,0,0,0,0,],[],[],[],[],[],[],[],]],
      [[1,2,3,4,5,6,7,8],[[0,0,0,0,0,0,0,0,],[],[],[],[],[],[],[],]],
      [[1,2,3,4,5,6,7,8],[[0,0,0,0,0,0,0,0,],[],[],[],[],[],[],[],]],
      [[1,2,3,4,5,6,7,8],[[0,0,0,0,0,0,0,0,],[],[],[],[],[],[],[],]],
      [[1,2,3,4,5,6,7,8],[[0,0,0,0,0,0,0,0,],[],[],[],[],[],[],[],]],
      [[1,2,3,4,5,6,7,8],[[0,0,0,0,0,0,0,0,],[],[],[],[],[],[],[],]],
      [[1,2,3,4,5,6,7,8],[[0,0,0,0,0,0,0,0,],[],[],[],[],[],[],[],]]
      ]
      
      
  def reset_png():#重置png
    global png
    png=[
    "00000000",
    "",
    "",
    "",
    "",
    "",
    "",
    ""
    ]
  def char( char, x, y, color=0xffff, background=0x0000):#通过framebuf取出字模
    global png
    global Letter
    global l
    w = 16
    buffer = bytearray(int(w*w/8))
    framebuffer = framebuf.FrameBuffer(buffer, w, w, framebuf.MONO_HLSB)
    print(char, bytearray(char))
    print("buf before", buffer)
    framebuffer.text(char, 0, 0)
    print("buf after", buffer)
    k=1
    for i in range(13):
      if i%2==0:
        print("buf ", i, buffer[i])
        a=(bin( buffer[i]))[2:]
        b=7-len(a)
        print(b)
        c='0'
        for i in range(b):#补0
          c=c+'0'
        a=c+a
        #print(a)
        
        png[k]=png[k]+a
        k=k+1
    

  def show_png(a):#改为line函数使用的格式
    global Letter
    global l
    temp=list(a)
    b=a[0]
    a=[]
    for i in range(8):
      for j in range(8):
        if temp[i][j]=="1":
          Letter[l][1][i].append(j)
    l=l+1

  def init():#右移
    global Letter
    temp=Letter
    #更改数组内的值并赋给temp
    for i in range(len(temp)):# 2 loops 选择字母
      for j in range(8):#8 loops 选择行
        for k in range(len(temp[i][1][j])):#每行的每个点加8
          temp[i][1][j][k]=temp[i][1][j][k]+8*i
    #值更改完毕，开始左移，每显示一个，左移一个单位，即所有的数字减一
    #还需加上判断，小于0则灭，等于0为亮，大于7位灭，等于7位亮
  def move():#每次左移一列
    global Letter
    temp=Letter
    for i in range(len(temp)):# 2 loops 选择字母
      for j in range(8):#8 loops 选择行
        for k in range(len(temp[i][1][j])):
          temp[i][1][j][k]=temp[i][1][j][k]-1
          #一个循环后，某行的所有的点向左移动一位
      #8个循环后，一个字母的所有点向左一位
    #show_LED(temp)

  def show_word(temp_show):#显示当前情况
    #temp = Letter[]
    for k in range(25):
      for i in range(len(temp_show)):
        for l in range(8):
          line(temp_show[i][0][l],temp_show[i][1][l])

  def show_LED(word):
    global png
    lword=list(word)
    for i in range(len(lword)):#给Letter赋值
      char(lword[i],100,100)
      show_png(png)
      reset_png()
    Letter[1]=[#在第二位加入心形
        [1,2,3,4,5,6,7, 8],
        [[],
        [1,2,4,5],
        [0,3,6],
        [0,6],
        [1,5],
        [2,4],
        [3],
        []]
      ]
    init()#右移
    for i in range(len(lword)*8+4):#将Letter按顺序右移
      move()#左移一列
      show_word(Letter)#显示
      pyb.delay(20)
    reser_Letter()#重置Letter

使用循环调用函数：
 ::

  while True:
	show_LED("I NEUAI")#最多输入8位


实验现象
^^^^^^^^^^^^^^^^^^^^^
运行程序可以看到一个滚动的I❤NEUAI在点阵上显示

.. image:: ../picture/ledmove.gif

