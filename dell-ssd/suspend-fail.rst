.. -*- coding: utf-8; mode: rst; -*-

.. index:: dell, linux

サスペンド後復帰できなくなる
============================

2024年11月4日

2024年11月6日 加筆

今回のSSD換装が原因かどうかわからないのですけども
サスペンド後復帰できなくなりました。電源長押しして強制電源OFFして電源ON〜ブートするハメになります。

ハイバネート禁止
----------------

2024年11月6日

.. code-block::

   [Sleep]
   MemorySleepMode=s2idle
   AllowSuspend=yes
   AllowHibernation=no
   AllowSuspendThenHibernate=no
   AllowHybridSleep=no
   #SuspendState=mem standby freeze
   #HibernateMode=platform shutdown
   #HibernateDelaySec=
   #SuspendEstimationSec=60min


結論…やっぱりアカンわ
----------------------

2024年11月6日 うーん、やっぱり復旧できなくなるので、これが原因ではないっぽい。元に戻しておく(下記 sleep.confの追加を削除)

s2idle を止めて deep にします。

 /etc/systemd/sleep.conf に以下を追加し再起動。

.. code::

   [Sleep]
   MemorySleepMode=deep

一時的にテストするには

.. code::

   sudo sh -c "echo deep > /sys/power/mem_sleep"

これは、 サスペンドは(成功すれば)何度でもできますが、電源OFFするとこの設定は消えます。

現状を確認するには

.. code::

   $ cat /sys/power/mem_sleep 
   s2idle [deep]

ここで、 表示されるのが選択可能な一覧で、 [ ] で囲まれている方が現在選択されている方です。

参考
----

`電源管理/サスペンドとハイバネート <https://wiki.archlinux.jp/index.php/%E9%9B%BB%E6%BA%90%E7%AE%A1%E7%90%86/%E3%82%B5%E3%82%B9%E3%83%9A%E3%83%B3%E3%83%89%E3%81%A8%E3%83%8F%E3%82%A4%E3%83%90%E3%83%8D%E3%83%BC%E3%83%88>`_
