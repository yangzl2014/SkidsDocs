:mod:`array` -- 数组
======================================

.. module:: array
   :synopsis: 数组

该模块实现相应CPython模块的子集，如下所示。更多信息，请参见：
|see_cpython_module| :mod:`python:array`.

支持格式编码： ``b``, ``B``, ``h``, ``H``, ``i``, ``I``, ``l``,
``L``, ``q``, ``Q``, ``f``, ``d`` （后两种依赖于浮点支持）。

类
-------

.. class:: array.array(typecode, [iterable])

    使用给定类型的元素创建数组。数组的初始内容由 `iterable` 给定。若未给定内容，则创建一个空数组。

    .. method:: append(val)

        将新元素添加到数组末尾，并不断增加。

    .. method:: extend(iterable)

        将包含在迭代中的新元素添加到数组末尾，并不断增加。
