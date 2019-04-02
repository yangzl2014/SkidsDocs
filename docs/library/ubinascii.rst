:mod:`ubinascii` -- 二进制/ASCII转换 
============================================

.. module:: ubinascii
   :synopsis: 二进制/ASCII转换 

该模块实现相应CPython模块的子集，如下所示。更多信息，请参见
|see_cpython_module| :mod:`python:binascii`.

该模块实现二进制数据和ASCII格式的各种编码之间的转换（双向）。

函数
---------

.. function:: hexlify(data, [sep])

   将二进制数据转换为十六进制表示。返回字节字符串。

   .. admonition:: Difference to CPython
      :class: attention

      若给定附加参数，则提供 ``sep`` ，用做十六进制值的分隔符。

.. function:: unhexlify(data)

   将十六进制数据转换为二进制表示。返回字节字符串。（即十六进制的倒数）

.. function:: a2b_base64(data)

   解码base64编码的数据，忽略输入中的无效字符。符合'rfc 2045 s.6.8<https://tools.ietf.org/html/rfc245 section-6.8>，返回字节对象。

.. function:: b2a_base64(data)

   以base64格式编码二进制数据，如'rfc 3548<https://tools.ietf.org/html/rfc3548.html>`。返回编码数据后跟换行符，作为字节对象。
   
   
  