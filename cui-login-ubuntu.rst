.. -*- coding: utf-8; mode: rst; -*-

.. N10JCがグラフィカルログイン後に固まる現象が多発したので
   CUIログインに変更する。サーバ用途なのでそもそもX要らない。
   なんでdesktop入れたのかって？Ubuntu-Serverには64bit版しか無かったからである。


Ubuntu Linux を CUI ログインに
==============================


そもそも正常にログインできてない時
----------------------------------

GRUBでlinux...の行のroの後ろ辺りに systemd.unit=multi-user.target を追加して起動。

.. note::
   参考
   
   Ubuntu 15.04 を GRUBメニューから CUI で起動する方法(防備録)
   
   http://qiita.com/ikwzm/items/5514b0fe9a8728e8aecb


正常にログインできているとき
----------------------------

| systemclt get-default

| sudo systemctl set-default multi-user.target 


.. note::
   参考
   
   systemd ターゲットでの作業

   https://access.redhat.com/documentation/ja-JP/Red_Hat_Enterprise_Linux/7/html/System_Administrators_Guide/sect-Managing_Services_with_systemd-Targets.html
