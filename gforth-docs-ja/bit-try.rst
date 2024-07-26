.. -*- coding: utf-8; mode: rst; -*-

.. index:: gforth

ちょいとお試しでDocker版を起動してみる
======================================

2024年7月27日

(未訳 INSTALL.md )

docker 版。 手元で試したのは Gforth 0.7.9_20240118 でした。

もってくる
----------

.. code:: bash
	  
   docker pull forthy42/gforth


起動
----

.. code:: bash

   alias gforthdk="docker run -ti --rm forthy42/gforth"

おまけ
------

起動後に dark-mode または light-mode でちょっぴりカラフルになります。
