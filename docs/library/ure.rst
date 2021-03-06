:mod:`ure` -- 正则表达式
========================================

.. module:: ure
   :synopsis: 正则表达式

该模块实现相应CPython模块的子集，如下所示。更多信息，请参见
|see_cpython_module| :mod:`python:re`.

该模块实现正则表达式操作。支持的正则表达式语法是CPython ``re`` 模块的一个子集（实际上是POSIX扩展正则表达式的子集）。

支持运营商为：

``'.'``
   匹配任何字符。

``'[]'``
   匹配字符集。支持单个字符和范围。

``'^'``

``'$'``

``'?'``

``'*'``

``'+'``

``'??'``

``'*?'``

``'+?'``

``'()'``
   Grouping. Each group is capturing (a substring it captures can be accessed
   with `match.group()` method).

不支持计数重复({m,n})、更高级断言、名称组等。


函数
---------

.. function:: compile(regex_str)

   编译正则表达式，返回 `regex` 对象。

.. function:: match(regex_str, string)

   将 `regex` 与 `string` 匹配。匹配通常从字符串的起始位置进行。

.. function:: search(regex_str, string)

   在 `string` 中搜索 `regex` 。与 `match` 不同，这将首先搜索与正则表达式相匹配的字符串（若正则表达式固定，则字符串为0）。

.. data:: DEBUG

   标记值，显示有关已编译表达式的调试信息。


.. _regex:

正则表达式对象
-------------

编译正则表达式。该类实例使用 `ure.compile()` 创建。

.. method:: regex.match(string)
            regex.search(string)

   与模块级函数 `match()` 和 `search()` 相似。若将同一正则表达式应用于多个字符串，则使用该方法会大大提高效率。

.. method:: regex.split(string, max_split=-1)

   使用正则表达式拆分字符串。若给定，则指定将拆分的最大数量。返回字符串列表（若指定，则可能会有多达 *max_split+1* 个元素）。

匹配对象
-------------

匹配由 `match()` 和 `search()` 方法返回的对象。

.. method:: match.group([index])

   Return matching (sub)string. *index* is 0 for entire match,
   1 and above for each capturing group. 
   返回匹配（子）字符串。若完全匹配 *index* 为0， 对于每个捕获组为1或更多。
   仅支持数字组。

