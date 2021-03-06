:mod:`ucollections` -- 容器
=====================================================

.. module:: ucollections
   :synopsis: 容器

该模块实现相应CPython模块的子集，如下所示。更多信息，请参见
|see_cpython_module| :mod:`python:collections`.

该模块实现高级集合和容器类型来保存/累积各种对象。

类
-------

.. function:: namedtuple(name, fields)

    这是一个用以创建具有特定名称和字段集的新命名的元组类型的函数。命名元组为元组的子类，
    它可以通过数值索引或具有符号字段名的属性访问语法来访问其字段。字段是指定字段名的字符串序列。
    为实现与CPython的兼容，也可为一个带有空格分隔字段的字符串（但是这样效率较低）。
    用法示例::

        from ucollections import namedtuple

        MyTuple = namedtuple("MyTuple", ("id", "name"))
        t1 = MyTuple(1, "foo")
        t2 = MyTuple(2, "bar")
        print(t1.name)
        assert t2.name == t2[1]

.. function:: OrderedDict(...)

    ``dict`` 类型的子类，该子类记住并保存所添加键的顺序。当有序字典重复时，键/项将按照其添加的顺序返回::

        from ucollections import OrderedDict

        # To make benefit of ordered keys, OrderedDict should be initialized 为充分利用有序键，应将有序字典初始化
        # from sequence of (key, value) pairs. 从序列对（键、值）中
        d = OrderedDict([("z", 1), ("a", 2)])
        # More items can be added as usual 可添加更多项
        d["w"] = 5
        d["b"] = 3
        for k, v in d.items():
            print(k, v)

    输出::

        z 1
        a 2
        w 5
        b 3
