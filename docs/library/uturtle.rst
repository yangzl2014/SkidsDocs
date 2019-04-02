:mod:`uturtle` -- 海龟画图
======================================================

.. module:: uturtle
   :synopsis: 海龟画图

海龟画图

class Color
-----------

.. class:: uturtle.Color(colorName)

    创建 Color 对象。参数为颜色名字，支持 'black'、'white'、'red'、'pink'、'gray'、'green'、<br>'lightgray'、'blue'、'cyan'、'magenta'、'yellow'、'darkgray'。

.. class:: uturtle.Color(r, g, b)

    创建 Color 对象。参数依次为：红色分量、绿色分量、蓝色分量。

class Vec2D
-----------

.. class:: uturtle.Vec2D(x, y)

    创建 Vec2D 对象。参数依次为：横坐标、纵坐标。

    .. method:: rotate(angle)

        旋转指定的角度。

    .. method:: rotate_rad(rad)

        旋转指定的弧度。

    .. method:: to_screen_coordinate()

        获取对应的屏幕坐标。

class Turtle
------------

.. class:: uturtle.Turtle()

    创建 Turtle 对象。

    .. method:: delay()

        获取延时值。

    .. method:: delay(delay)

        设置延时值。

    .. method:: position()

        获取当前位置。

    .. method:: pos()

        获取当前位置。

    .. method:: forward(distance)

        前进指定的距离。

    .. method:: fd(distance)

        前进指定的距离。

    .. method:: goto(vec2D)

        前进到指定的位置。

    .. method:: goto(x, y)

        前进到指定的位置。

    .. method:: right(angle)

        右转指定的角度。

    .. method:: rt(angle)

        右转指定的角度。

    .. method:: left(angle)

        左转指定的角度。

    .. method:: lt(angle)

        左转指定的角度。

    .. method:: heading()

        获取当前方向对应的角度。

    .. method:: setheading(to_angle)

        设置当前方向对应的角度。

    .. method:: seth(to_angle)

        设置当前方向对应的角度。

    .. method:: pendown()

        落下画笔。

    .. method:: pd()

        落下画笔。

    .. method:: penup()

        抬起画笔。

    .. method:: pu()

        抬起画笔。

    .. method:: begin_fill()

        开始填充。

    .. method:: end_fill()

        结束填充。

    .. method:: filling()

        获取当前是否在填充状态。

    .. method:: color(colorName)

        设置画笔颜色和填充颜色为 colorName 指定的颜色。

    .. method:: color(penColorName, fillColorName)

        设置画笔颜色为 penColorName 指定的颜色，填充颜色为 fillColorName 指定的颜色。

    .. method:: color(r, g, b)

        设置画笔颜色和填充颜色为 Color(r, g, b)。

    .. method:: pencolor()

        获取画笔颜色。

    .. method:: pencolor(color)

        设置画笔颜色为 color。

    .. method:: pencolor(colorName)

        设置画笔颜色为 colorName 指定的颜色。

    .. method:: pencolor(r, g, b)

        设置画笔颜色为 Color(r, g, b)。

    .. method:: bgcolor()

        获取背景颜色。

    .. method:: bgcolor(colorName)

        设置背景颜色为 colorName 指定的颜色。

    .. method:: bgcolor(r, g, b)

        设置背景颜色为 Color(r, g, b)。

    .. method:: pensize()

        获取当前画笔大小。

    .. method:: pensize(size)

        设置当前画笔大小。

    .. method:: speed()

        获取当前速度。

    .. method:: speed(speed_str)

        设置当前速度。str 可以为 'fastest'、'fast'、'normal'、'slow'、'slowest'。

    .. method:: speed(speed)

        设置当前速度。参数值为0~10的整数，包含0与10。

    .. method:: fillcolor()

        获取填充颜色。

    .. method:: fillcolor(colorName)

        设置填充颜色为 colorName 指定的颜色。

    .. method:: fillcolor(r, g, b)

        设置填充颜色为 Color(r, g, b)。

    .. method:: circle(radius, extent, steps)

        画圆。参数依次为：圆的半径、圆的角度、圆的边数。

    .. method:: towards(vec2D)

        获取当前位置到指定点所在的射线与初始正方向的夹角。

    .. method:: towards(x, y)

        获取当前位置到指定点所在的射线与初始正方向的夹角。

    .. method:: home()

        回到原点。

    .. method:: reset()

        重置为初始状态。

    .. method:: clear()

        清屏。
