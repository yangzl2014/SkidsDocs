:mod:`screen` -- 屏幕绘制
======================================================

.. module:: screen
   :synopsis: 屏幕绘制

提供屏幕绘制的基础函数

静态方法
--------

.. function:: clear()

   清屏

.. function:: drawline(x1, y1, x2, y2, pensize, pencolor)

   画线段。参数依次为：起点横坐标、起点纵坐标、终点横坐标、终点纵坐标、画笔宽度、画笔颜色。

.. function:: drawpoly(point_list, fillcolor)

   填充多边形。参数依次为：多边形顶点列表、填充颜色。
