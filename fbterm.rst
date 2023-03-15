.. -*- coding: utf-8; mode: rst; -*-

.. N10JC本体をいじるときに備えて日本語表示できるようにしておく。
   

コンソールで日本語表示
======================

2016年03月16日

videoグループに属してないと使えないので,まずadduserして、
一旦ログオフ、ログオン(一旦ロフオフしないとadduserは反映されない)

.. code-block:: sh

   $ sudo adduser <<username>> video

インストール。

.. code-block:: sh

   $ apt-get install fbterm unifont
   $ LANG=C
   $ fbterm -s 16
   $ LANG=ja_JP.UTF-8

LANG=Cしてからfbterm立ち上げると文字が欠けない…なんで？

.. code-block:: sh

   $ sudo chown <<username>>:<<group>> ~/.fbtermrc

参考
....

fbterm を使う（Ubuntu 13.10）
   
http://pulpdust.org/item/1702
   
fbtermのインストール・設定及びその他まとめ
   
https://banken07.wordpress.com/2013/02/19/fbterm%E3%81%AE%E3%82%A4%E3%83%B3%E3%82%B9%E3%83%88%E3%83%BC%E3%83%AB%E3%83%BB%E8%A8%AD%E5%AE%9A%E5%8F%8A%E3%81%B3%E3%81%9D%E3%81%AE%E4%BB%96%E3%81%BE%E3%81%A8%E3%82%81/

コンソールでCaps LockキーをCtrlにする
-------------------------------------

/etc/default/keyborad の

	XKBOPTIONS=""

を

	XKBOPTIONS="ctrl:nocaps" にして、

.. code-block:: sh

   $ sudo dpkg-reconfigure -phigh console-setup

でOK。

参考
....

CapsLockをCtrlにするまとめ

http://lambdalisue.hatenablog.com/entry/2013/09/27/212118

の、「console-setupを利用した方法」参照。
