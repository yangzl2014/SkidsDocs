:mod:`ubitmap` -- 绘制位图
======================================================

.. module:: ubitmap
   :synopsis: 绘制位图

在屏幕上绘制位图，位图数据来自于预先编码后的文件，也可以是固件支持的图片，包括："shitou"、"jiandao"、"bu"。

class Bitmap
--------------------

.. class:: ubitmap.Bitmap(name)

    创建位图对象。参数为固件支持的图片名称，包括："shitou"、"jiandao"、"bu"。

    .. method:: draw(x, y)

        绘制位图。参数依次为：位图在屏幕上的横坐标、位图在屏幕上的纵坐标。

class BitmapFromFile
--------------------

.. class:: ubitmap.BitmapFromFile(path)

    创建位图对象。参数为预先编码后的位图路径。

    .. method:: is_valid()

        位图对象是否有效。

    .. method:: destroy()

        销毁位图对象。位图对象销毁后，不能再进行绘制操作。

    .. method:: width()

        获取位图宽度。若位图对象无效，返回 None。

    .. method:: height()

        获取位图高度。若位图对象无效，返回 None。

    .. method:: draw(x, y)

        绘制位图。参数依次为：位图在屏幕上的横坐标、位图在屏幕上的纵坐标。位图对象无效时返回 False，否则返回 True。
