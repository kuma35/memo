.. -*- coding: utf-8; mode: rst; -*-


ユーザに/ttyUSB0の書き込み権限を付与
====================================

2016年03月16日

(ubuntu 15.10 i386)ユーザhideoをdialoutグループに追加する。

.. code-block:: bash

   $ sudo adduser hideo dialout

ログインしなおす。
ログインしなおさないと反映されないので注意！！

