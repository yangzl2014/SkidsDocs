.. _introduction:

简要介绍
=======================

程序说明
-----------------------

EndDevice_source:
  + EndATRT.py：终端上AT指令收发器，用于解析和封装AT指令
  + end_player.py：包含猜拳类Guess、连接类Connect、主程序

Gateway_source:
  + GatATRT.py：网关上AT指令收发器，用于解析和封装AT指令
  + gat_player.py：包含猜拳类Guess、连接类Connect、主程序

使用说明
-----------------------

首先修改end_player.py、gat_player.py中的PANID，要唯一，以建立自己的ZigBee网，防止与其他人冲突。
  + 一块Skids上的ZigBee烧写EndDevice固件作为终端设备，一块Skids上的ZigBee烧写Coordinator固件作为网关设备
  + 将EndDevice_source两个程序下载到终端Skids板上，将Gateway_source两个程序下载到网关Skids板上
  + 先重启网关Skids，运行主程序；再重启终端Skids，运行主程序；然后两个Skids之间就可以进行猜拳游戏
  + 注：如果要修改设置的PANID，需先重启skids，再执行主程序。

  .. image:: img/guess.jpg
    :alt: guess
    :width: 640px
