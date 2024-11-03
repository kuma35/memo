.. -*- coding: utf-8; mode: rst; -*-

.. index:: dell, linux

サスペンド後復帰できなくなる
============================

2024年11月4日

今回のSSD換装が原因かどうかわからないのですけども
サスペンド後復帰できなくなりました。電源長押しして強制電源OFFして電源ON〜ブートするハメになります。

結論
----

s2idle を止めて deep にします。

 /etc/systemd/sleep.conf に以下を追加します。

.. code::

   [Sleep]
   MemorySleepMode=deep

一時的にテストするには

.. code::

   sudo sh -c "echo deep > /sys/power/mem_sleep"

現状を確認するには

.. code::

   $ cat /sys/power/mem_sleep 
   s2idle [deep]

表示されるのが選択可能な一覧で、 [ ] で囲まれている方が現在選択されている方です。
