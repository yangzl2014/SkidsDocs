打开LED和基本的Python概念
=========================================

在trailbreaker上最简单的方法是打开连接到电路板的LED。 连接电路板，并按照教程1中的说明登录。我们将从解释器中的转动和LED开始，键入以下内容 ::

    >>> myled = pyb.LED(1)
    >>> myled.on()
    >>> myled.off()

这些命令可以打开和关闭LED。

这一切都很好，但我们希望这个过程是自动化的。 在您喜欢的文本编辑器中打开trailbreaker上的文件MAIN.PY。 将以下行写入或粘贴到文件中。 如果您是python的新手，那么请确保缩进正确，因为这很重要！::

    led = pyb.LED(2)
    while True:
        led.toggle()
        pyb.delay(1000)

保存时，trailbreaker上的红灯应亮约一秒钟。要运行脚本，请执行软重置（CTRL-D）。然后trailbreaker将重新启动，您应该看到绿灯持续闪烁。成功，是你建立邪恶机器人军队的第一步！当您对烦人的闪光灯感到厌倦时，请按下终端上的CTRL-C以停止运行。
 
那么这段代码有什么用呢？首先，我们需要一些术语。 Python是一种面向对象的语言，几乎python中的所有东西都是*类*，当你创建一个类的实例时，你得到一个*对象*。类具有与它们相关联的*方法*。方法（也称为成员函数）用于与对象交互或控制对象。

第一行代码创建了一个LED对象，然后我们称之为led对象。当我们创建对象时，它需要一个参数，该参数必须在1到4之间，对应于电路板上的4个LED。 pyb.LED类有三个我们将使用的重要成员函数：on（），off（）和toggle（）。我们使用的另一个函数是pyb.delay（），这只是等待一个给定的时间，以毫秒为单位。一旦我们创建了LED对象，声明为True：创建一个无限循环，在打开和关闭之间切换LED并等待1秒。

**练习：尝试改变切换LED和打开不同LED之间的时间。**

**练习：直接连接到trailbreaker，创建一个pyb.LED对象并使用on（）方法打开它。**

你的trailbreaker上的迪斯科舞厅
-----------------------

到目前为止，我们只使用了一个LED，但是trailbreaker有4个可用。 让我们首先为每个LED创建一个对象，以便我们可以控制每个LED。 我们通过创建具有列表理解的LEDS列表来实现这一点。::

    leds = [pyb.LED(i) for i in range(1,5)]

如果使用不是1,2,3,4的数字调用pyb.LED（），您将收到错误消息。
接下来，我们将设置一个无限循环，循环通过每个LED打开和关闭它们。 ::

    n = 0
    while True:
      n = (n + 1) % 4
      leds[n].toggle()
      pyb.delay(50)

这里，n跟踪当前LED，每次循环执行时我们循环到下一个n（％符号是模数运算符，保持n在0和3之间。）然后我们访问第n个LED并切换它。如果你运行它，你应该看到每个LED都打开然后所有LED按顺序再次关闭。

您可能会发现的一个问题是，如果您停止脚本然后再次启动它，那就是LED从上一次运行中停留，破坏了我们精心编排的迪斯科舞厅。我们可以通过在初始化脚本然后使用try / finally块关闭所有LED来解决这个问题。当您按CTRL-C时，MicroPython会生成VCPInterrupt异常。异常通常意味着出现了问题，您可以使用try：命令来“捕获”异常。在这种情况下，只是用户中断脚本，因此我们不需要捕获错误，只是告诉MicroPython退出时要做什么。 finally块执行此操作，我们使用它来确保所有LED都关闭。完整的代码是::

    leds = [pyb.LED(i) for i in range(1,5)]
    for l in leds: 
        l.off()

    n = 0
    try:
       while True:
          n = (n + 1) % 4
          leds[n].toggle()
          pyb.delay(50)
    finally:
        for l in leds:
            l.off()

特殊LED
----------------

黄色和蓝色LED是特殊的。 除了打开和关闭它们，您还可以使用intensity（）方法控制它们的强度。 这需要一个0到255之间的数字来确定它的亮度。 以下脚本使蓝色LED逐渐变亮，然后再将其关闭。 ::

    led = pyb.LED(4)
    intensity = 0
    while True:
        intensity = (intensity + 1) % 255
        led.intensity(intensity)
        pyb.delay(20)

您可以在LED 1和2上调用intensity（），但它们只能关闭或打开。 0将它们关闭，最多255个任何其他数字将它们打开。