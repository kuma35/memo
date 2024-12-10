.. -*- coding: utf-8; mode: rst; -*-

.. index:: git

git ドキュメント翻訳環境構築
============================

2024年12月11日

フォルダ構成
------------

make ; make install

したときに以下にgitの実行ファイルがセットされる。

- ${HOME}/bin
- ${HOME}/share/git-core
- ${HOME}/share/git-gui
- ${HOME}/share/gitk
- ${HOME}/share/gitweb


手元では ${HOME}/work/ 以下に展開している

- git 毎回空にして github 公式(?)リポジトリ から git clone で持ってくる。
- git.X.XX git をリネームした旧フォルダ
- git-docs-ja git フォルダを upstream として git pull upstream master してから、新しいブランチへ適用する。

手順
----

https://github.com/git/git.git から git clone してきます。
その前に、現行の git フォルダを退避します。後ろの数字は当時のバージョン番号です。

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
