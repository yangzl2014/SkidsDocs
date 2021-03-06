:mod:`math` -- 数学函数
=====================================

.. module:: math
   :synopsis: 数学函数

该模块实现相应CPython模块的子集，如下所示。更多信息，请参见
|see_cpython_module| :mod:`python:math`.

 ``math`` 模块提供一些用于处理浮点数的基本函数。

注意: 在pyboard和OpenMV Cam中，浮点数的精度为32位。

函数
---------

.. function:: acos(x)

   返回 ``x`` 的反余弦函数。

.. function:: acosh(x)

   返回 ``x`` 的反双曲余弦函数。

.. function:: asin(x)

   返回 ``x`` 的反正弦函数。

.. function:: asinh(x)

   返回 ``x`` 的反双曲正弦函数。

.. function:: atan(x)

   返回 ``x`` 的反正切函数。

.. function:: atan2(y, x)

   返回 ``y/x`` 的反正切函数的主值。

.. function:: atanh(x)

   返回 ``x`` 的反双曲正切函数。

.. function:: ceil(x)

   返回一个正整数,将 ``x`` 向正无穷大舍入。

.. function:: copysign(x, y)

   以 ``y`` 的符号返回 ``x`` 。

.. function:: cos(x)

   返回 ``x`` 的的余弦函数。

.. function:: cosh(x)

   返回 ``x`` 的双曲余弦函数。

.. function:: degrees(x)

   返回弧度 ``x`` 对应的度数。

.. function:: erf(x)

   返回 ``x`` 的误差函数。

.. function:: erfc(x)

   返回 ``x`` 的补差函数。

.. function:: exp(x)

   返回 ``x`` 的指数。

.. function:: expm1(x)

   返回 ``exp(x) - 1``.

.. function:: fabs(x)

   返回 ``x`` 的绝对值。

.. function:: floor(x)

   返回一个整数,将 ``x`` 向负无穷大舍入.

.. function:: fmod(x, y)

   返回 ``x/y`` 的余数。

.. function:: frexp(x)

   将浮点数分解为尾数和指数。返回值是元组 ``(m, e)`` ，确切表示为 ``x == m * 2**e`` 。
   若 ``x == 0`` ，则该函数返回 ``(0.0, 0)`` ，否则 ``0.5 <= abs(m) < 1`` 关系成立。

.. function:: gamma(x)

   返回 ``x`` 的伽玛函数。

.. function:: isfinite(x)

   若 ``x`` 为有限的，则返回 ``True`` 。

.. function:: isinf(x)

   若 ``x`` 为无限，则返回 ``True`` 。

.. function:: isnan(x)

   若 ``x`` 非数字，则返回 ``True`` 。

.. function:: ldexp(x, exp)

   返回 ``x * (2**exp)``.

.. function:: lgamma(x)

   返回 ``x`` 的伽玛函数的自然对数。

.. function:: log(x)

   返回 ``x`` 的自然对数。

.. function:: log10(x)

   返回 ``x`` 以10为底的对数。

.. function:: log2(x)

   返回 ``x`` 以2为底的对数。

.. function:: modf(x)

   返回包含两个浮点值的元组，即x的分数和积分部分。两个返回值都与 ``x`` 有同样标记。

.. function:: pow(x, y)

   将 ``x`` 返回 ``y`` 的幂。

.. function:: radians(x)

   返回度数 ``x`` 的弧度。

.. function:: sin(x)

   返回 ``x`` 的正弦函数。

.. function:: sinh(x)

   返回的 ``x`` 双曲正弦函数。

.. function:: sqrt(x)

   返回 ``x`` 的平方根.

.. function:: tan(x)

   返回 ``x`` 的正切函数。

.. function:: tanh(x)

   返回 ``x`` 的双曲正切函数。

.. function:: trunc(x)

   截尾函数，返回 ``x`` 的整数部分。

常量
---------

.. data:: e

   自然对数的底。

.. data:: pi

   一个圆的周长与其直径的比值。
