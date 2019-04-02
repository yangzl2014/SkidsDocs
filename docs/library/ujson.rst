:mod:`ujson` -- JSON编码与解码
==========================================

.. module:: ujson
   :synopsis: JSON编码与解码

该模块实现相应 `CPython` 模块的子集，如下所示。更多信息，请参见
|see_cpython_module| :mod:`python:json`.

该模块允许Python对象和JSON数据格式之间的转换。

函数
---------

.. function:: dumps(obj)

   返回表示为JSON字符串的 ``obj`` 。

.. function:: loads(str)

   解析JSON ``str`` 并返回一个对象。若该字符串未正确排列，则会引发示值误差。

.. function:: load(stream)

   解析给定的*stream*，将其解释为JSON字符串并将数据反序列化为Python对象。结果对象需要返回。

   分析将继续，直到遇到文件结尾。如果*stream*中的数据格式不正确，则会引发`ValueError`。

   Parsing continues until end-of-file is encountered.
   A :exc:`ValueError` is raised if the data in *stream* is not correctly formed.

.. function:: loads(str)

   分析JSON *str*并返回一个对象。如果字符串格式不正确，则引发：exc:`ValueError`。
   
