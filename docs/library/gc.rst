:mod:`gc` -- 控制垃圾回收器
==========================================

.. module:: gc
   :synopsis: 控制垃圾回收器

该模块实现相应CPython模块的子集，如下所示。更多信息，请参见:
|see_cpython_module| :mod:`python:gc`.

函数
---------

.. function:: enable()

   启用垃圾自动收集。

.. function:: disable()

   禁用垃圾自动收集。堆内存仍可分配，仍可使用 `gc.collect()` 手动启动垃圾回收。

.. function:: collect()

   运行垃圾回收。

.. function:: mem_alloc()

   返回分配的堆RAM的字节数量。

.. function:: gc.mem_free()

   .. admonition:: 与CPython区别
      :class: attention

      此函数为MicroPython扩展。

.. function:: mem_free()

   返回可用的堆RAM的字节数量。

   .. admonition:: 与CPython区别
      :class: attention

      此函数为MicroPython扩展。

.. function:: threshold([amount])

   设置或查询额外的GC分配阈值。通常，仅在无法满足新分配时，才会触发一个集合，即内存不足（OOM）时。若调用该函数，除OOM之外，
   每次分配字节数之后都会触发一个集合（总之，自前一段时间后，已分配了相当数量的字节）。数量通常被指定为少于满堆大小，
   其目的是在堆耗尽前触发一个集合，且希望前期集合能够防止过度内存碎片。这是一种启发式的度量方法，其效果随应用程序不同而不同，
   其数量参数的最优值也不尽相同。

   无参数调用该函数将返回阈值的当前值。-1值表示禁用配置阈值。

   .. admonition:: 与CPython区别
      :class: attention

      该函数为MicroPython的扩展。CPython有一个类似函数 ``set_threshold()`` ，但由于不同的GC实现，其签名与语义也不同。
