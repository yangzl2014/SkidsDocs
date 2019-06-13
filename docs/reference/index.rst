MicroPython语言
========================

MicroPython旨在实现语言语法方面的Python 3.4标准（具有未来版本中所指定的特征），
MicroPython的大多数特征与
`docs.python.org <https://docs.python.org/3/reference/index.html>`_.中的《语言参考》文件中所述特征相同。

MicroPython标准库在相应章节中进行了描述。
MicroPython与CPython的区别描述了MicroPython
与CPython的不同之处（主要与标准库和类型相关，也与语言级特征相关）。

本章描述了MicroPython实现的特性和使用的最佳方法。

.. toctree::
   :maxdepth: 1

   glossary.rst
   repl.rst
   isr_rules.rst
   speed_python.rst
   constrained.rst
   packages.rst

.. only:: port_pyboard

   .. toctree::
      :maxdepth: 1

      asm_thumb2_index.rst
