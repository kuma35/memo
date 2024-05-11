.. -*- coding: utf-8; mode: rst; -*-

.. index:: emacs; grep-find

emacs grep-find サンプル集
==========================

2024年05月12日

pythonやperl内で使うのに慣れていて、シェル内で使うgrepの正規表現
に慣れていないので。
例えばgrepの場合、繰り返しの ``+`` の前にはバックスラッシュが必要だったり。

msgstr..'
---------

msgstr "'

msgstr の文字列の先頭が ' (クォーテーションマーク) なのを抽出。
'hogehoge' は info では _hogehoge_ となってしまうため、 ` (バッククォート) に置換するための調査

.. code-block:: text

   find . -type f -name \*.po -exec grep --color -nH -e "msgstr..'" {} +

\([A-Z]+\)
----------

カレントディレクトリ以下の \*.po ファイルより
丸括弧で囲まれた英大文字2つ以上の文字列を取得。
例えば、 ``(AC)`` 、  ``(CDC)`` など。

.. code-block:: text

   find . -type f -name \*.po -exec grep --color -nH -e "([[:upper:]]\{2,\})" {} +

応用 [(;][A-Z]+\)
.................

``(interface association descriptor;IAD)`` という表記を引っ張ってくる。

.. code-block:: text

   find . -type f -exec grep --color -nH -e "[(;][[:upper:]]\{2,\})" {} +

\.[[:lower:]]\{3\}\W
--------------------

2020年06月07日

ピリオド(.)で始まり小文字3文字で終わるのを取ってくる(拡張子等)

.. code-block:: text
   
   find . -type f -name \*.rst -exec grep --color -nH -e "\.[[:lower:]]\{3\}\W" {} +


0b[01]+
-------

2020年06月14日

独自の2進数形式 0b0101... を探してくる。

.. code-block:: text

   find . -type f -name \*.rst -exec grep --color -nH -e "0b[01]\+"  {} +

\-[[:upper:]]
-------------

2020年06月15日

Kindleでハイフネーションをつなげた時に、ハイフンの後が大文字だと
ハイフンを残したまま繋げる事があり、それを検出するため。

.. code-block:: text

   find . -type f -name \*.rst -exec grep --color -nH -e"\-[[:upper:]]"  {} +
