.. -*- coding: utf-8; mode: rst; -*-

.. index:: gforth

ちょいとお試しでDocker版を起動してみる
======================================

2024年12月20日

docker 版。 手元で試したのは Gforth 0.7.9_20241009 でした。

もってくる
----------

.. code:: bash
	  
   docker pull forthy42/gforth


起動
----

.. code:: bash

   $ docker run -ti --rm forthy42/gforth


おまけ
------

起動後に dark-mode または light-mode でちょっぴりカラフルになります。
