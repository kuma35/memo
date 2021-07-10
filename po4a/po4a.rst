.. -*- coding: utf-8; mode: rst; -*-

.. index:: po-mode; html; po4a

静的HTMLの翻訳
==============

2021年07月10日

経緯
----

libusbの文書を翻訳することにした。libusbの文書は、

- doxygenを使ってソースファイル(C言語)から生成する。
- doxygen様々な形式で出力できるがlibusbはhtml出力。
- 翻訳するとなると、ソースファイル(のコメント)または出力をいじる事になる。
- ソースファイル改変なんかしたくないのでhtmlをいじることにする。

翻訳ツール
----------

po4a
....

gettext - po なプロセスをプログラム言語以外でもやりたい！という代物。htmlもOK。今回はコレを利用した。

omegaT
......

GUIなツール。各種有料・無料サービスとも連携可能なようです。
以前もhtmlを翻訳したことあるけれども、致命的な問題なく扱うことができる。
内蔵エディタはキーバインドとかナニソレなのでそこの部分の作業効率はあがらない。
(sed文書翻訳に使用)

GNUN
....

GNUnited Nations

gnu.orgのドキュメント翻訳用。
フォルダがgnu.org専用に固定されちゃってるとか、gnu.orgへ投稿前提とか、正に専用ツールなのでそれ以外の用途には使いにくい。

google翻訳者ツールキット
........................

(webサービス) 既にサービス終了。

sphinx-intl
...........

sphinx文書であれば問題なく翻訳を行える。
reStructureのディレクティブ等もちゃんと認識した上で原文切り出ししてくれる
(Scrapy文書、pyusb文書、Thinking Forth翻訳に使用)。

pandoc
......

翻訳ツールじゃなくて形式変換だけども。

LaTex → reStructure に使用したが、変換不能な部分が多く散々であった(Thinking Forth翻訳に利用)。

翻訳サイト
----------

翻訳そのものは google翻訳 や weblio辞書 で済ませています。必要に応じて更にググります。


po4a
----

通常プログラムソースファイルで使われる gettext を、html等様々な形式のテキストでも使えるようにしようという試み。らしい。

po4aをそのまま使う訳ではない
............................

私は原文1ファイルに対して翻訳作業用のファイル(po)も1つという対応で作業したいんである。
しかしpo4aは原文ファイル全てを1つのpoファイルにまとめちゃうんである。
言語毎に1つのpoファイルという発想のようである。

よってpo4a統合コマンド(とpo4a.cfg)は使わずに、個々のコマンドをリッチgettextとして呼び出すのである。

作業環境
--------

- sudo apt install doxygen libusbでドキュメントを生成するために必要(配布物にはドキュメントは含まれていないため手元で生成する必要有る)
- sudo apt install libudev-dev autogen.sh実行時に必要

libusbのバイナリをコンパイルしなくても doc/Makefile の実行だけでドキュメントは生成できる。


フォルダ構成
............

- ./ Makefile等
- source/api-1.0 原文ファイル群(html,js,css)
- po poファイル群
- docs/ 訳文書(翻訳を適用したhtml、jsとcssはsourceからコピー)

原文を用意する
--------------

libusbはautogen.shを実行すると各フォルダにMakefileが生成される。

docディレクトリに入り、(docディレクトリの) Makefile を実行すると doc/api-1.0 に文書が出来る。

今回は api-1.0/* を libusb翻訳作業用の source/ にコピーして使用。

初回・原文ファイル追加時のみの作業
----------------------------------

初回または原文ファイルが追加された時。

初回専用スクリプト
....................


.. code-block:: sh

   #!/bin/sh
   find ./source/libusb.sourceforge.io/api-1.0/ -name "*.html" -printf "po4a-gettextize --format xhtml --master %p --master-charset utf-8 --copyright-holder \"libusb\" --package-name \"libusb\" --package-version \"0.0\" --po ./po/%f.po\n"

.. _updatepo_script:

原文ファイル追加時用スクリプト
..............................

.. code-block:: sh

   po4a-gettextize --format xhtml --master source/api-1.0/functions_vars.html --master-charset UTF-8  --copyright-holder libusb --package-name "libusb-api-doc" --package-version "1.0" --po po/functions_vars.html.po

いままで存在していなかった functions_vars.html が追加になった時のスクリプト

POヘッダ編集
------------

生成した全てのPOファイルについて、POヘッダを設定する必要がある。
設定しないとPOファイルコンパイルやpo4a-updatepo(msgmerge)でエラーになる。

.. code-block:: text
		
   # -*- coding: utf-8 -*-
   # SOME DESCRIPTIVE TITLE
   # Copyright (C) YEAR libusb
   # This file is distributed under the same license as the libusb package.
   # FIRST AUTHOR <EMAIL@ADDRESS>, YEAR.
   #
   msgid ""
   msgstr ""
   "Project-Id-Version: libusb-api-doc 1.0\n"
   "POT-Creation-Date: 2021-07-09 02:36+0900\n"
   "PO-Revision-Date: 2021-07-09 02:54+0900\n"
   "Last-Translator: kuma35\n"
   "Language-Team: Japanese\n"
   "Language: ja_JP\n"
   "MIME-Version: 1.0\n"
   "Content-Type: text/plain; charset=UTF-8\n"
   "Content-Transfer-Encoding: 8bit\n"

- #コマンドでコメント編集にしておまじないを追加 -*- coding: utf-8 -*-
- Last-Translator Language-Team Language Content-Type を表記の通りとする。

翻訳
----

poファイルを編集する。emacs po-modeなど

.. _translate:

訳文書生成
----------

make html

Makefileの詳細は下記 :ref:`makefile` 参照。

「80％に達していないので…破棄」
................................

po4a-translateのkeepオプション(--keep)のデフォルトが80に設定されているため。
`--keep 0` と設定すれば翻訳の進捗に関わらず訳文書を生成するようになる。

.. _updatepo:

原文更新
--------

make updatepo

Makefileの詳細は下記 :ref:`makefile` 参照。

なお、原文に新しいファイルが追加された場合、make html でエラーとなるので上記 :ref:`updatepo_script` を参考にpoファイルを追加する。

.. _makefile:

Makefile
--------

:ref:`translate` や :ref:`updatepo` で使うMakefile

.. code-block:: make

   # SRC_DIR = source/libusb.sourceforge.io/api-1.0
   SRC_DIR = source/api-1.0
   PO_DIR = po
   TARGET_DIR = docs
   
   SRC_ALL = $(wildcard $(SRC_DIR)/*)
   
   SRC_HTML = $(filter %.html,$(SRC_ALL))
   SRC_OTHER = $(filter-out %.html,$(SRC_ALL))
   
   PO_PO = $(wildcard $(PO_DIR)/*.html.po)
   
   TARGET_HTML = $(addprefix $(TARGET_DIR)/,$(notdir $(SRC_HTML)))
   TARGET_OTHER = $(addprefix $(TARGET_DIR)/,$(notdir $(SRC_OTHER)))
   
   a_file = $(addsuffix $(3),$(addprefix $(1)/,$(notdir $(basename $(2)))))
   
   
   html: $(TARGET_HTML) $(TARGET_OTHER)
   
   updatepo: $(PO_PO)
   
   # static pattern
   $(TARGET_HTML): $(TARGET_DIR)/%.html : $(PO_DIR)/%.html.po
      po4a-translate -v --format xhtml --master $(call a_file,$(SRC_DIR),$@,.html) --master-charset UTF-8 --po $? --localized $(call a_file,$(TARGET_DIR),$@,.html) --localized-charset UTF-8 --keep 0

   $(TARGET_OTHER): $(TARGET_DIR)/% : $(SRC_DIR)/%
      cp -v -u $? $@

   $(PO_PO): $(PO_DIR)/%.html.po : $(SRC_DIR)/%.html
      po4a-updatepo --format xhtml --master $? --master-charset UTF-8 --previous --copyright-holder "libusb" --package-name "libusb" --package-version "1.0" --po $@

   .PHONY: html updatepo

   
