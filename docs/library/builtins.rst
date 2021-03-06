内置函数
================================

此处描述了所有内置函数和异常，这些函数和异常也可通过 ``builtins`` 模块获得。

函数和类型
-------------------

.. function:: abs()

.. function:: all()

.. function:: any()

.. function:: bin()

.. class:: bool()

.. class:: bytearray()

.. class:: bytes()

    |see_cpython_module| `python:bytes`.

.. function:: callable()

.. function:: chr()

.. function:: classmethod()

.. function:: compile()

.. class:: complex()

.. function:: delattr(obj, name)

   参数名称应为一个字符串，且此函数从 *obj* 给出的对象中删除指定的属性。

.. class:: dict()

.. function:: dir()

.. function:: divmod()

.. function:: enumerate()

.. function:: eval()

.. function:: exec()

.. function:: filter()

.. class:: float()

.. class:: frozenset()

.. function:: getattr()

.. function:: globals()

.. function:: hasattr()

.. function:: hash()

.. function:: hex()

.. function:: id()

.. function:: input()

.. class:: int()

   .. classmethod:: from_bytes(bytes, byteorder)

      在MicroPython中， `byteorder` 参数须为位置参数（与CPython兼容）。

   .. method:: to_bytes(size, byteorder)

      在MicroPython中， `byteorder` 参数须为位置参数（与CPython兼容）。

.. function:: isinstance()

.. function:: issubclass()

.. function:: iter()

.. function:: len()

.. class:: list()

.. function:: locals()

.. function:: map()

.. function:: max()

.. class:: memoryview()

.. function:: min()

.. function:: next()

.. class:: object()

.. function:: oct()

.. function:: open()

.. function:: ord()

.. function:: pow()

.. function:: print()

.. function:: property()

.. function:: range()

.. function:: repr()

.. function:: reversed()

.. function:: round()

.. class:: set()

.. function:: setattr()

.. class:: slice()

   内置 *slice* 为切片对象所具有的类型。

.. function:: sorted()

.. function:: staticmethod()

.. class:: str()

.. function:: sum()

.. function:: super()

.. class:: tuple()

.. function:: type()

.. function:: zip()


Exceptions
----------

.. exception:: AssertionError

.. exception:: AttributeError

.. exception:: Exception

.. exception:: ImportError

.. exception:: IndexError

.. exception:: KeyboardInterrupt

.. exception:: KeyError

.. exception:: MemoryError

.. exception:: NameError

.. exception:: NotImplementedError

.. exception:: OSError

    |see_cpython_module| `python:OSError` .MicroPython并未实现 ``errno`` 属性，而是使用标准方式来获取异常参数 ``exc.args[0]`` 。

.. exception:: RuntimeError

.. exception:: StopIteration

.. exception:: SyntaxError

.. exception:: SystemExit

    |see_cpython_module| `python:SystemExit`.

.. exception:: TypeError

    |see_cpython_module| `python:TypeError`.

.. exception:: ValueError

.. exception:: ZeroDivisionError
