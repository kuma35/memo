.. -*- coding: utf-8; mode: rst; -*-

.. index:: emacs; grep-find

emacs grep-find サンプル集
==========================

2020年05月31日

pythonやperl内で使うのに慣れていて、シェル内で使うgrepの正規表現
に慣れていないので。
例えばgrepの場合、繰り返しの ``+`` の前にはバックスラッシュが必要だったり。

\([A-Z]+\)
----------

カレントディレクトリの \*.po ファイルより
丸括弧で囲まれた英大文字2つ以上の文字列を取得。
例えば、 ``(AC)`` 、  ``(CDC)`` など。

.. code-block:: text

   find . -type f -name \*.po -exec grep --color -nH -e "([[:upper:]]\{2,\})" {} +

応用 [(;][A-Z]+\)
.................

``(interface association descriptor;IAD)`` という表記を引っ張ってくる。

.. code-block:: text

   find . -type f -exec grep --color -nH -e "[(;][[:upper:]]\{2,\})" {} +
