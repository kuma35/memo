.. -*- coding: utf-8; mode: rst; -*-

.. index:: emacs; po-mode; helm; void

emacs po-mode Symbol’s function definition is void: po-with-temp-buffer
=======================================================================

2021年09月18日

Ubuntu 20.04.2 LTSにアップグレードしたら
GNU Emacs 26.3 (build 2, x86_64-pc-linux-gnu, GTK+ Version 3.24.14) of 2020-03-26,
modified by Debian でPOファイルを開いたときに

.. code-block:: text

   and: Symbol’s function definition is void: po-with-temp-buffer

とエラーが出てファイル読み込めなくなった。

.emacs.d/intl.el に

.. code-block:: lisp

   (require 'po)
   (require 'po-mode)

追加したら動くようになった。これが正しい対処方法かどうかは不明。

(Emacs21.2以降にはpo.elって名前でpo-compat.el相当のファイルがバンドルされている)
