.. -*- coding: utf-8; mode: rst; -*-

ROSインストール
===============

for Ubuntu 16.04 LTS(Xenial) i386
---------------------------------

それぞれのバージョンが、ディストリビューションの特定のバージョンでしかインストールできないことに注意。

当該ディストリビューション用ソースから作成する場合も同様に束縛される。

ROSに合わせてOSのバージョンを選ぶ必要がある。


Ubuntu 16.04 LTS(Xenial) i386 なら、選べるので最新のはKinetic

2018/4/2 shadow-repository 時点で使うと

.. code-block:: bash

   $ sudo apt update
   $ sudo apt list ^ros | less
   $ sudo apt list | grep ^ros | less
   $ sudo apt install ros-kinetic-desktop-full
