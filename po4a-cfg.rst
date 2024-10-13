.. -*- coding: utf-8; mode: rst; -*-

.. index:: po4a; po4a-updatepo; po4a-translate; po4a.cfg

po4a-updatepo や po4a-translate から po4a + po4a.cfg に移行
===========================================================

2024年10月14日

経緯
----

従来は po4a-updatepo や po4a-translate を使ってきたが、
po4a 0.68 から非推奨となったので po4a + po4a.cfg を使うようにした。

結論
----

翻訳ファイルごとにそれぞれ po4a.cfg を用意してください。
doc/foo.texi → doc-ja/foo.texi, doc/bar.texi → doc-ja/bar.texi ならば、
doc-po/po4a-cfg/foo.texi.cfg, doc-po/po4a-cfg/bar.texi.cfg を用意します。

フォルダ構成
------------

.. code-block::
   
   project
   +
   +- doc           原文フォルダ
   +- doc-po        po,翻訳用Makefile,翻訳用スクリプト
   +   +- pot
   +   +- po4a-cfg
   +- doc-ja        原文フォルダのコピー＋翻訳済ファイル

プログラムの文書は割と doc/ 内に文書生成用の Makefile が配置してあって、
文書生成用Makefileは親フォルダ内のファイルを参照することしばしば(たとえば、
親フォルダ内のプログラム・ソース・ファイル内のコメント部分を参照するなど)であるので、
doc/ と同じレベルに翻訳済用フォルダ doc-ja/ を配置し、
doc/Makefile を doc-ja/Makefile にコピーするだけで使えるようにしてある。


po4a.cfg例
----------

doc-po/po4a-cfg/vmgen.texi.cfg

.. code-block::
   
   # generate by mk-po4a-cfg.sh 2024年 10月 9日 水曜日 08:02:21 JST
   [po4a_langs] ja
   [po4a_paths] ./pot/$master.pot $lang:./$master.po
   [type: texinfo] ../doc/vmgen.texi $lang:../doc-ja/vmgen.texi


これは原文が ../doc/vmgen.texi で、 ja への翻訳を行い、
pot ファイルは ./pot/vmgen.texi.pot , po ファイルは ./vmgen.texi.po , 翻訳済ファイルは ../doc-ja/vmgen.texi で、
文書の形式は texinfo を使うということを示している。

$master
.......

po4a.cfg の $master は マスターファイルの basename に置換される。つまり上記では vmgen.texi である。
vmgen ではないので注意。

これはどういうことになるかというと、 pot ファイルや po ファイルは自動的に vmgen.texi.pot や vmgen.texi.po となる。
vmgen.pot や vmgen.po じゃないんである。

make では \*.c → \*.o とかするので make とは相性悪いのだけど、 これが po4a の流儀ということのようである。
よってこれに対応して Makefile も書き換えた。

$lang
.....

今回は ja に置換される。複数有る場合はそれぞれ置換したコピーを作成する

.. code-block::
   
   [po4a_langs] ja fr

ならｂ

.. code-block::
   
   [po4a_paths] ./pot/$master.pot ja:./$master.po
   [po4a_paths] ./pot/$master.pot fr:./$master.po
   [type: texinfo] ../doc/vmgen.texi ja:../doc-ja/vmgen.texi
   [type: texinfo] ../doc/vmgen.texi fr:../doc-ja/vmgen.texi

と同じ、って事だそう。詳しくは po4a のドキュメント見て下さい。

mk-po4a-cfg.sh
..............

doc-po/Makefile 内から起動

.. code-block:: bash

   #!/usr/bin/sh
   # mk-po4a-cfg.sh <<src-pathfile(relative)>> <<pot-path(relative)>> <<lang(e.g: ja)>> <<po-path(relative)>> <<dst-pathfile(relative)>>
   SRC_FILE=$1
   POT_PATH=$2
   LANGCODE=$3
   PO_PATH=$4
   DST_FILE=$5
   echo "# generate by $0" `date`
   echo "[po4a_langs] ${LANGCODE}"
   echo "[po4a_paths] ${POT_PATH}/\$master.pot \$lang:${PO_PATH}/\$master.po"
   echo "[type: texinfo] ${SRC_FILE} \$lang:${DST_FILE}"

   

Makefile例
----------

doc-po/Makefile

.. code-block:: Makefile

   LANG_CODE = ja
   SRC_DIR = ../doc
   SRC_EXT = .texi
   POT_DIR = ./pot
   PO_DIR = .
   PO_EXT = .texi.po
   DST_DIR = ../doc-ja
   PO4A = po4a
   PO4A_CFG_DIR = ./po4a-cfg
   PO4A_CFG_EXT = .texi.cfg
   IGNORE = version.texi fdl.texi gpl.texi

   # 2024/05/09
   # version.texi, fdl.texi は 翻訳対象から外す(doc/ から /doc-ja へ直にcp)
   SRC_PACKAGE = $(filter-out $(IGNORE),$(notdir $(wildcard $(SRC_DIR)/*$(SRC_EXT))))

   SRC_FILES = $(addprefix $(SRC_DIR)/,$(SRC_PACKAGE))

   $(warning SRC_FILES = $(SRC_FILES))

   PO4A_CFG_FILES = $(addprefix $(PO4A_CFG_DIR)/,$(SRC_PACKAGE:$(SRC_EXT)=$(PO4A_CFG_EXT)))

   $(warning PO4A_CFG_FILES = $(PO4A_CFG_FILES))

   PO_FILES = $(addprefix $(PO_DIR)/,$(SRC_PACKAGE:$(SRC_EXT)=$(PO_EXT)))

   $(warning PO_FILES = $(PO_FILES))

   DST_FILES = $(addprefix $(DST_DIR)/,$(SRC_PACKAGE))

   $(warning DST_FILES = $(DST_FILES))

   # $(error "debug stop")

   $(PO4A_CFG_FILES): $(PO4A_CFG_DIR)/%$(PO4A_CFG_EXT) : $(SRC_DIR)/%$(SRC_EXT)
	bash mk-po4a-cfg.sh $< $(POT_DIR) $(LANG_CODE) $(PO_DIR) $(addprefix $(DST_DIR)/,$(notdir $<)) > $@
	cat $@

   $(PO_FILES): $(PO_DIR)/%$(PO_EXT) : $(SRC_DIR)/%$(SRC_EXT) $(PO4A_CFG_FILES)
	po4a --verbose --no-translations --keep 0 --master-charset UTF-8 $(addprefix $(PO4A_CFG_DIR)/,$(notdir $(@:$(PO_EXT)=$(PO4A_CFG_EXT))))


   $(DST_FILES): $(DST_DIR)/%$(SRC_EXT) : $(PO_DIR)/%$(PO_EXT) $(PO4A_CFG_FILES)
	po4a --verbose --no-update --keep 0 --master-charset UTF-8 $(addprefix $(PO4A_CFG_DIR)/,$(notdir $(@:$(SRC_EXT)=$(PO4A_CFG_EXT))))

   # doc/Makefile に 追加した場合に反映させる。
   $(DST_DIR)/Makefile : $(SRC_DIR)/Makefile
	cp $< $@

   ja: $(DST_FILES) $(DST_DIR)/Makefile

   # rsyncの $(SRC_DIR)の直後の / 重要。超重要
   # cmds-*.txt are templates. not need translation. copy from $(SRC_DIR) to $(DST_DIR)
   # rsync -av --exclude "*.txt" $(SRC_DIR)/ $(DST_DIR)
   clean:
	find $(DST_DIR) -type f | xargs rm -f
	for fname in ChangeLog dir_sample Makefile Makefile.in trampvar.text.in ;do cp $(SRC_DIR)/$fname $(DST_DIR)/ ;done

   # .PHONEY: ja clean

   all: ja

poファイル移行
--------------

いままで gforth.po , vmgen.po で作業していたので、1回だけ実行。

原文ファイル名と翻訳済ファイル名は変更無いが、今回、 po ファイル名は変更となる。
ちょいと走らせると pot や cfg とともに(全然翻訳出来てない gforth.texi.po , vmgen.texi.po が出来てる。)

.. code-block:: bash
		
   po4a --verbose --no-translations --keep 0 --master-charset UTF-8 po4a-cfg/gforth.texi.cfg
   po4a --verbose --no-translations --keep 0 --master-charset UTF-8 po4a-cfg/vmgen.texi.cfg

\*.po から \*.texi.po へ移行

.. code-block:: bash
		
   msgmerge gforth.po pot/gforth.texi.pot -o gforth.texi.po
   msgmerge vmgen.po pot/vmgen.texi.pot -o vmgen.texi.po

   mv gforth.texi.po gforth.po
   mv vmgen.texi.po vmgen.po

   git mv gforth.po gforth.texi.po
   git mv vmgen.po vmgen.texi.po

utf8 と UTF-8
-------------

従来、 --master-charset utf8 としてきたが、今回から --master-charset UTF-8 とした。
po4aの中の人(perl)的には utf8 は昔からの割とアバウトなやつで、 UTF-8 と書くといまどきの
UTF-8 解釈してくれるそうなんである。

運用
----

ご参考まで

doc-po/Makefile を呼び出す doc-po/compile.sh があって、 日々の作業ではコレを使う。

info だけ作るときは compile.sh
html も作るときは compile.sh html
これらを emacs の M-x compile から呼び出して使っている

.. code-block:: bash

   #!/bin/bash
   PROJ=${HOME}/work/gforth-docs-ja
   BRANCH=docs-ja-0
   cd ${PROJ}/doc-po
   make ja
   exitcode=$?
   if [ ${exitcode} -ne 0 ]; then
	notify-send --hint=int:resident:0 -u critical gforth-docs-ja "doc-po/Makefile エラー"
	exit ${exitcode}
   fi
   cd ${PROJ}/doc-ja

   for no_translate_file in Makefile Makefile.in fdl.texi gpl.texi version.texi gforth.css gforth.js ; do
	cp --update=none ../doc/${no_translate_file} .
   done

   # 2024.05.08
   # make から ターゲット ps はとりあえず外す。エラー出たので。
   # l.2: Unicode char @u8:こ not defined for Texinfo
   # make info html txt
   make info $*
   exitcode=$?
   if [ ${exitcode} -ne 0 ]; then
	notify-send --hint=int:resident:0 -u critical gforth-docs-ja "doc-ja/Makefile エラー"
	exit ${exitcode}
   fi

   # info 用 dir ファイル生成
   rm -f dir
   install-info --info-file=gforth.info --dir-file=dir
   install-info --info-file=vmgen.info --dir-file=dir

   # cp htmls
   cp -rp --update ${PROJ}/doc-ja/gforth/* ${PROJ}/docs/${BRANCH}/gforth/
   cp -rp --update ${PROJ}/doc-ja/vmgen/* ${PROJ}/docs/${BRANCH}/vmgen/

   # restore htmls and manpaese in Documentation-ja
   #${PROJ}/doc-po/restore-htmls.sh

   notify-send -u normal gforth-docs-ja "compile完了。"


