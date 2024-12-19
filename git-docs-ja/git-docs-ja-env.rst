.. -*- coding: utf-8; mode: rst; -*-

.. index:: git

git ドキュメント翻訳環境構築
============================

- 更新:2024年12月20日
- 作成:2024年12月14日

フォルダ構成
------------

翻訳環境は手元では ${HOME}/work/ 以下に展開している

- ${HOME}/work/git 毎回空にして github 公式(?)リポジトリ から git clone で持ってくる。
- ${HOME}/work/git.X.XX git をリネームした旧フォルダ
- ${HOME}/work/git-docs-ja 翻訳プロジェクト本体。 ${HOME}/work/git フォルダを upstream として git pull upstream master してから、新しいブランチへ適用する。

現行のを退避:

.. code-block:: bash

   $ cd ${HOME}/work
   $ mv git git.X.XX

最新版を取ってくる:

.. code-block:: bash

   $ git clone https://github.com/git/git.git

実行ファイルを作る:

.. code-block:: bash

   $ cd ${HOME}/work/git
   $ make
   $ make install

make install したときに以下にgitの実行ファイルがセットされる。 黙って上書きされる。

- ${HOME}/bin
- ${HOME}/share/git-core
- ${HOME}/share/git-gui
- ${HOME}/share/gitk
- ${HOME}/share/gitweb

日常利用するには ${HOME}/bin を PATH の先頭に加える。
  

asciidoc最新版を使う
--------------------

10.2.1 を使うため。

ディストリのバージョンが 10.2.0 だったので

 SyntaxWarning: invalid escape sequence '\S'

が出まくる。 代わりに 10.2.1 を使う

.. code-block:: bash

   $ cd Documentation-po
   $ mkdir venv
   $ python3 -m venv venv

.. code-block:: bash

   $ source venb/bin/activate
   $ pip install -U pip
   $ pip install asciidoc

Documentation-po/compile.sh に 追記

.. code-block:: bash

   #!/usr/bin/bash
   (中略)
   # using python3 venv for asciidoc
   source ${PROJ}/Documentation-po/venv/bin/activate

source は sh では動かなかったのでシェバングは bash にしてください。
   
po4a.cfg対応(docs-ja-3→docs-ja-4)
----------------------------------

:doc:`../po4a-cfg-2` 

複数版表示対応(docs-ja-3→docs-ja-4)
------------------------------------

TODO:

翻訳中の最新版だけではなくて、過去の翻訳版も並行して表示できるように compile.sh / Makefile や docs/ の index.html 等をいじります。

docs/<BRANCH>/hogehoge

古いブランチのは基本的にはあまり動かないので、 もし更新があれば都度手動で最新のところで反映させるものとします(でいいだろうか？？)
持ってくるのは git checkout でコミット指定してパス指定して持ってくる形かな？ 古い版は docs/hogehoge の形なので直接は持って来れない。

翻訳更新開始手順
----------------

元が新しいバージョンになったら、新しいブランチ git-docs-9999 を作って翻訳の方も更新します。

新しいブランチを切るのは翻訳の古い版も残しておきたいからです。


https://github.com/git/git.git から git clone してくる訳ですが、
その前に、現行の git フォルダを退避します。 後ろの数字は当時のバージョン番号です。

.. code-block:: bash

   cd ~/work
   mv git git.2.28
   git clone https://github.com/git/git.git

最新版のバイナリを得るために make します。
ここでは ${HOME}/bin に入れたいので以下のようにします。 他に入れたい場所があれば INSTALL ファイルをご覧ください。

${HOME}/bin の既存のは黙って上書きされるので注意してください。

.. code-block:: bash

   cd git
   make
   make install

次に翻訳用のフォルダへ移動します。 翻訳用のフォルダは手元では ${HOME}/work/git-docs-ja です。
そして、まずは master へ upsteram (上記gitフォルダから)更新を取り込みます。

.. code-block:: bash
   
   cd ~/work/git-docs-ja
   git switch master
   git pull upstream master

いままでの先端である docs-ja-n のその先から docs-ja-n+1 を分岐させる。
例えば今までの先端が docs-ja-3 であったならば、 その先端から docs-ja-4 を分岐させます:

.. code-block:: bash

   $ git switch docs-ja-3
   Updating files: 100% (3777/3777), done.
   Switched to branch 'docs-ja-3'

   $ git status
   ブランチ docs-ja-3
   nothing to commit, working tree clean

   $ git branch docs-ja-4
   $ git switch docs-ja-4
   Switched to branch 'docs-ja-4'

mastar ブランチから、 新しいブランチ(docs-ja-4)に取り込みます。
翻訳時に原文にも手を入れている箇所があるため、 CONFLICT がいくつも発生すると思います。

.. code-block:: bash

   $ git merge master


CONFLICTをすべて修正し、 compile.sh が通るようになったら、 一旦翻訳を生成してみます。

.. code-block:: bash

   $ git merge --continue


デフォルトブランチ変更(github)
------------------------------

githubの当該リポジトリの Setting で、 デフォルトリポジトリを変更します。
また、 pages の持ってくる元のブランチも変更します。

これをやらないと古いのが表示され続けます。

Makefile例
----------

翻訳作業用 Makefile : Documentation-po/Makefile

.. code-block:: make

   # $(BRANCH) specify make argument
   SRC_DIR = ../Documentation
   SEDOUT_DIR = ../Documentation-sedout
   SEDOUT_EXT = .txt
   ASCIIDOC_EXT = .txt
   PO_EXT = .po
   POT_DIR = pot
   PO4A_CFG_EXT = .po4cfg
   DST_DIR = ../Documentation-ja
   TECHNICAL_DIR = technical
   RELNOTES_DIR = RelNotes
   CONFIG_DIR = config
   HOWTO_DIR = howto
   INCLUDES_DIR = includes
   MERGETOOLS_DIR = mergetools
   SRC_SUB_DIRS = $(TECHNICAL_DIR) $(RELNOTES_DIR) $(CONFIG_DIR) $(HOWTO_DIR) $(INCLUDES_DIR) $(MERGETOOLS_DIR)
   SRC_INSTALL_TEXT = ../INSTALL
   PO_INSTALL_TEXT = INSTALL.po
   PO4A_CFG_INSTALL_TEXT = INSTALL.po4cfg
   DST_INSTALL_TEXT = ../docs/$(BRANCH)/INSTALL.txt
   # cmds_txt and mergetools_txt from Documentation/Makefile there are templates. not need translation.
   cmds_txt = cmds-ancillaryinterrogators.txt \
      cmds-ancillarymanipulators.txt \
      cmds-mainporcelain.txt \
      cmds-plumbinginterrogators.txt \
      cmds-plumbingmanipulators.txt \
      cmds-synchingrepositories.txt \
      cmds-synchelpers.txt \
      cmds-guide.txt \
      cmds-purehelpers.txt \
      cmds-foreignscminterface.txt
   mergetools_txt = mergetools-diff.txt mergetools-merge.txt
   EXCLUDE_TXT = $(cmds_txt) $(mergetools_txt)
   SRC_PACKAGE = $(filter-out $(EXCLUDE_TXT),$(notdir $(wildcard $(SRC_DIR)/*$(ASCIIDOC_EXT))))
   SRC_PACKAGE += $(foreach sub_dir, $(SRC_SUB_DIRS), $(addprefix $(sub_dir)/,$(notdir $(wildcard $(SRC_DIR)/$(sub_dir)/*$(ASCIIDOC_EXT)))))
   SRC_FILES = $(addprefix $(SRC_DIR)/,$(SRC_PACKAGE))
   SEDOUT_FILES = $(addprefix $(SEDOUT_DIR)/,$(SRC_PACKAGE:$(ASCIIDOC_EXT)=$(SEDOUT_EXT)))
   PO_FILES = $(SRC_PACKAGE:$(ASCIIDOC_EXT)=$(PO_EXT))
   PO4A_CFG_FILES = $(SRC_PACKAGE:$(ASCIIDOC_EXT)=$(PO4A_CFG_EXT))
   DST_FILES = $(addprefix $(DST_DIR)/,$(SRC_PACKAGE))

   help:
      @echo "instead, please run from compile.sh"

   .DEFAULT_GOAL := help

   $(PO4A_CFG_FILES): %$(PO4A_CFG_EXT) : $(SEDOUT_DIR)/%$(ASCIIDOC_EXT)
      ./mk-po4a-cfg.sh $< > $@

   $(PO4A_CFG_INSTALL_TEXT): $(SRC_INSTALL_TEXT)
      ./mk-po4a-cfg-install-text.sh $< $(DST_INSTALL_TEXT) > $@

   $(SEDOUT_FILES): $(SEDOUT_DIR)/%$(SEDOUT_EXT) : $(SRC_DIR)/%$(ASCIIDOC_EXT)
      sed -f ./protect-opt-dash.sed < $< > $@

   $(PO_FILES): %$(PO_EXT) : $(SEDOUT_DIR)/%$(SEDOUT_EXT)
      po4a --no-translations --keep 0 --master-charset UTF-8 $(@:$(PO_EXT)=$(PO4A_CFG_EXT))

   $(PO_INSTALL_TEXT): $(SRC_INSTALL_TEXT)
      po4a --no-translations --keep 0 --master-charset UTF-8 $(@:$(PO_EXT)=$(PO4A_CFG_EXT))

   $(DST_FILES): $(DST_DIR)/%$(ASCIIDOC_EXT) : %$(PO_EXT) %$(PO4A_CFG_EXT)
      po4a --no-update --keep 0 --master-charset UTF-8 $(<:$(PO_EXT)=$(PO4A_CFG_EXT))

   $(DST_INSTALL_TEXT): $(PO_INSTALL_TEXT)
      po4a --no-update --keep 0 --master-charset UTF-8 $(<:$(PO_EXT)=$(PO4A_CFG_EXT))

   # Documentation/Makefile に 追加した場合に反映させる。 (technicalに一部html化されてないのがあった)
   $(DST_DIR)/Makefile : $(SRC_DIR)/Makefile
      cp $< $@

   # Documentation/technical/api-index.sh を変更した場合に反映させる。
   $(DST_DIR)/technical/api-index.sh : $(SRC_DIR)/technical/api-index.sh
      cp $< $@

   ja: $(DST_FILES) $(DST_INSTALL_TEXT) $(DST_DIR)/Makefile $(DST_DIR)/technical/api-index.sh

   # rsyncの $(SRC_DIR)の直後の / 重要。超重要
   # cmds-*.txt are templates. not need translation. copy from $(SRC_DIR) to $(DST_DIR)
   # rsync -av --exclude "*.txt" $(SRC_DIR)/ $(DST_DIR)
   clean:
      find $(DST_DIR) -type f | xargs rm -f
      find $(DST_DIR) -empty | xargs rmdir
      rsync -av --include "cmds-*.txt" --include "mergetools-*.txt" --exclude "*.txt" $(SRC_DIR)/ $(DST_DIR)

   .PHONEY: ja clean help

全体制御用 compile.sh
---------------------

中で source を使っているのでシェバングは #!/usr/bin/bash 指定しています。

make だけだと info を作ります。 make html とすると info と html を作ります。

下記は docs-ja-4 ブランチ用のため BRANCH=docs-ja-4 を指定しています。
これは github pages 公開用のフォルダ docs/ 以下に収める時に使用します。
翻訳済ファイル群は docs/docs-ja-4 以下に配置されます。

Documentation-po/compile.sh

.. code-block:: bash

   #!/usr/bin/bash
   PROJ=${HOME}/work/git-docs-ja
   BRANCH=docs-ja-4
   cd ${PROJ}/Documentation-sedout
   for dst_dir in technical RelNotes config howto includes mergetools
   do
      if [ ! -d ${dst_dir} ]; then
         mkdir ${dst_dir}
      fi
   done
   cd ${PROJ}/Documentation-po
   # using python3 venv for asciidoc
   source ${PROJ}/Documentation-po/venv/bin/activate

   make ja BRANCH=${BRANCH}
   exitcode=$?
   if [ ${exitcode} -ne 0 ]; then
      notify-send -u critical git-docs-ja "Documentation-po/Makefile エラー"
      exit ${exitcode}
   fi
   cd ${PROJ}/Documentation-ja
   make info $*
   exitcode=$?
   if [ ${exitcode} -ne 0 ]; then
      notify-send -u critical git-docs-ja "Documentation-ja/Makefile エラー"
      exit ${exitcode}
   fi
   # gen file "dir" for info. and publish to docs/info/
   make -f ${PROJ}/Documentation-po/publish-info.mak BRANCH=${BRANCH}
   exitcode=$?
   if [ ${exitcode} -ne 0 ]; then
      notify-send -u critical git-docs-ja "publish-info.mak エラー"
      exit ${exitcode}
   fi
   # restore htmls and manpaese in Documentation-ja
   ${PROJ}/Documentation-po/restore-htmls.sh
   ${PROJ}/Documentation-po/restore-manpages.sh
   # for github pages
   DIFF=diff ${PROJ}/Documentation-po/install-webdoc-only-html.sh ${PROJ}/docs/${BRANCH}/htmldocs
   gawk -f ${PROJ}/Documentation-po/publish-index.awk TEMPLATE=${PROJ}/Documentation-po/index.html.template OUTPUT=${PROJ}/docs/${BRANCH}/index.html < ${PROJ}/../git/GIT-VERSION-FILE
   exitcode=$?
   if [ ${exitcode} -ne 0 ]; then
      notify-send -u critical git-docs-ja "publish-index.awk エラー"
      exit ${exitcode}
   fi
   # restore htmls in docs
   cd ${PROJ}/docs/${BRANCH}
   ${PROJ}/Documentation-po/restore-htmls.sh
   #
   notify-send -u normal git-docs-ja "compile完了。"
