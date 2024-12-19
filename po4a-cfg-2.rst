.. -*- coding: utf-8; mode: rst; -*-

.. index:: po4a; po4a-updatepo; po4a-translate; po4a.cfg

po4a-updatepo や po4a-translate から po4a + po4a.cfg に移行その2
================================================================

2024年12月15日

:doc:`po4a-cfg` にて hoge.txt → hoge.txt.po のように、 .po 追加
となる方式で使って見たところ、 make との相性は大変悪かった。

なので、 hoge.txt → hoge.po のように、
単純に拡張子を付け替えるだけで使えるようにしてみた。

フォルダ構成
------------

「git 付属ドキュメント翻訳」は以下フォルダ構成になっている。

.. code-block::
   
   project
   +
   +- Documentation         「原文フォルダ」
   +- Documentation-sedout  「sed 用作業フォルダ」
   +- Documentation-po      「翻訳作業用フォルダ」 po, po4a.cfg
   +   +- pot               「POTフォルダ」
   +
   +- Documentation-ja      「翻訳済フォルダ」

プログラムの文書は割と「原文フォルダ」内に文書生成用の Makefile が配置してあって、
文書生成用Makefileは親フォルダ内のファイルを参照することしばしば(たとえば、
親フォルダ内のプログラム・ソース・ファイル内のコメント部分を参照するなど)であるので、
「原文フォルダ」と同じレベルに「翻訳済フォルダ」を配置し、 翻訳しないファイル達や
Makefile を 「翻訳済フォルダ」 にコピーするだけで使えるようにしてある。

make で扱いやすいように po と po4a.cfg ファイルを一緒に置くようにした。

po4a.cfg例
----------

「翻訳作業用フォルダ」 の Makefile にて 自動生成する。

以下は原文 Documentation/git-am.txt に対する po4a.cfg で、

=============================== ==================
ファイル名                      機能
=============================== ==================
Documentation/git-am.txt        原文ファイル
Documentation-sedout/git-am.txt sed 処理済ファイル
Documentation-po/git-am.po      翻訳用ファイル
Documentation-po/git-am.po4cfg  po4a.cfg ファイル
Documentation-po/pot/git-am.pot 翻訳用POTファイル
Documentation-ja/git-am.txt     翻訳済ファイル
=============================== ==================

.. code-block::

   # generate by ./mk-po4a-cfg.sh 2024年 12月 15日 日曜日 12:49:38 JST
   [po4a_langs] ja
   [type: asciidoc] ../Documentation-sedout/git-am.txt $lang:../Documentation-ja/git-am.txt
   [po4a_paths] pot/git-am.pot ja:git-am.po

- "[po4a_langs]" 和訳のみのため ja 固定。 他言語もある場合は追記。
- "[type: asciidoc]" ASCIIDOCの文書だから。
- "[type: asciidoc] 「マスター文書ファイル・パス名」 $lang:「翻訳済ファイル・パス名」"
- "[po4a_paths] 「POTファイル・パス名(拡張子 pot)」 ja:「翻訳作業用ファイル・パス名(拡張子 po)」

パスは make ( po4a ) を実行するフォルダ(ここでは Documentation-po ) からの相対指定である。

$master
.......

po4a.cfg の $master は マスターファイルの basename に置換される。つまり上記では git-am.txt である。
git-am ではないので注意。

これはどういうことになるかというと、 $master 使っちゃうと pot ファイルや po ファイルは git-am.txt.pot や git-am.txt.po となる。
git-am.pot や git-am.po じゃないんである。

make では \*.c → \*.o とかするので make とは相性悪いので、 $master を使わずにファイル名を指定するようにした。

mk-po4a-cfg.sh
..............

:doc:`po4a-cfg` とは中身違うので注意。 Documentation-po フォルダ内で実行する。

- ../Documentation 原文フォルダ
- ../Documentation-ja 翻訳済フォルダ
  
.. code-block:: bash

   #!/usr/bin/sh
   # mk-po4a-cfg.sh <<src-pathfile(relative)>>
   SRC_FILE=$1
   BASE_FILE=${SRC_FILE#../Documentation-sedout/}
   DST_FILE=../Documentation-ja/${BASE_FILE}
   BASE_BODY=${BASE_FILE%.txt}
   echo "# generate by $0" `date`
   echo "[po4a_langs] ja"
   echo "[type: asciidoc] ${SRC_FILE} \$lang:${DST_FILE}"
   echo "[po4a_paths] pot/${BASE_BODY}.pot ja:${BASE_BODY}.po"
