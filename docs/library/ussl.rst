:mod:`ussl` -- SSL/TLS 模块
=============================

.. module:: ussl
   :synopsis: TLS/SSL 套接字对象的包装

|see_cpython_module| :mod:`python:ssl`.

此模块提供对传输层安全性（以前被广泛称为“安全套接字层”）的访问，并为客户端和服务器端的网络套接字提供对等身份验证功能。

函数
---------

.. function:: ussl.wrap_socket(sock, server_side=False, keyfile=None, certfile=None, cert_reqs=CERT_NONE, ca_certs=None)

   接受一个'stream`*sock*（通常是'sock\u stream``类型的usocket.socket实例），并返回ssl.sslsocket的一个实例，该实例将底层流包装在一个SSL环境。返回的对象具有常见的“stream”接口方法，如
    `` read（）``，`` write（）``等。在Micropython中，返回的对象不公开套接字接口和方法，如``recv（）``，``send（）``。尤其是服务器端的SSL套接字应该从从返回的普通套接字创建。
   ：meth:`~usocket.socket.accept（）`在非SSL侦听服务器套接字上。

   取决于特定的底层模块实现` Micropyhon端口`，上面的某些或所有关键字参数可能不受支持。

.. warning::

   `ussl`模块的某些实现不验证服务器证书，这使得建立的SSL连接容易受到中间人攻击。

例外
----------

.. data:: ssl.SSLError

   此异常并不存在，而是使用它的基类OSError。

常量
---------

.. data:: ussl.CERT_NONE
          ussl.CERT_OPTIONAL
          ussl.CERT_REQUIRED

    支持的*cert_reqs*参数值。
	
	
	
	
	
	
