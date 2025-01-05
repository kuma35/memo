.. -*- coding: utf-8; mode: rst; -*-

.. index:: asciidoc, git

AsciiDoc の \`...\` が単語単位に分割される
==========================================

- 作成:2025年1月4日

現象
----

git のドキュメントは AsciiDoc で記述されています。

以前(Ubuntu 22.04LTS)の頃はバック・クォーテーション(\`)で囲った文字列全体を monospace font にレンダリングしてくれていました。

.. code-block:: text

   \`git version\` → html: <code>git version</code> info: @samp{git version}

今回(Ubntu 24.04LTS, asciidoc 10.2.0, git 2.47.1.404.ge66fd72e97) では単語単位に分割してしまうようになりました。

.. code-block:: text

   \`git version\` → html: <code>git</code> <code>version</code> info: @samp{git} @samp{version}

htmlでは見かけ上、 単語が monospace font に変わるだけで見かけは git version のままなのですが、
infoでは 'git' 'version' と表記されてしまいます。

回避策
------

htmlだけを生成する文書については、 はhtmlソース上では分けて表記されていますが見かけは変わらないので対処しません。

infoも生成する文書について以下の対処を行います。

空白を含まない単語・フレーズは変化無いので特に対処の必要はありません。

空白を含むフレーズは \` (バック・クォーテーション) ではなくて + (プラス記号) で囲えばOK。
AsciiDocでの扱いは違いますが、 手元では実害は無さそう。たぶん。

\` ... \` → \`+ ... +\`

.. code-block:: text

   +git version+ → html: <code>git version</code> info: @samp{git version}


対策
----

変更対象抽出
............

\`...\` を抽出します。 単一の単語ではなくてフレーズを検出します。単語間の区切りはスペースの他に
`<` , `>` , `[` , `]` です。

\*.po ファイルでは msgid と msgstr の両方に現れるのですが、 これを区別して取得するのは無理だったので、
とりあえずひっくるめて検索します。

info 生成しない、 html 文書だけのものは実害が無いので、 howto と technical と RelNotes は対象から外します。

.. code-block:: bash

   $ find ./ -not \( -path './howto/*' -o -path './technical/*' -o -path './RelNotes/*' \) -name "*.po" | xargs grep -n  -P '`(?:[^` ]+(?:[][<>]|\s)+){1,}[^` ]+`' | wc -l
   6636

おおよそ、 この半分ぐらい？

対象ファイル一覧は TODO リストにしたいので gen-enclose-back-tick-list.sh を使います enclose-back-tick-list.txt を生成します。
2回目以降の実行は別ファイルに出力して Emacs M-x ediff-files 等して diff 〜 merge してください。

.. code-block:: bash

   $ sh ./gen-enclose-back-tick-list.sh > enclose-back-tick-list.txt

.. code-block:: text
   :caption: enclose-back-tick-list.txt(抜粋)
   :name: enclose-back-tick-list.txt

   # -*- mode: org -*-
   # please see gen-enclose-back-tick-list.sh
   # toggle todo C-c C-t
   \* TODO ./MyFirstContribution.po	95  # html only
   \* TODO ./MyFirstObjectWalk.po	66  # html only
   \* TODO ./ReviewingGuidelines.po	4  # html only
   \* DONE ./blame-options.po	12
   \* TODO ./config.po	12
   \* TODO ./config/add.po	4
   \* TODO ./config/advice.po	4
   (後略)

.. code-block:: bash
   :caption: gen-enclose-back-tick-list.sh
   :name: gen-enclose-back-tick-list.sh

   #!/bin/sh
   find ./ -not \( -path './howto/*' -o -path './technical/*' -o -path './RelNotes/*' \) -name "*.po" | xargs grep -c  -P '`(?:[^` ]+(?:[][<>]|\s)+){1,}[^` ]+`' | sort | gawk 'BEGIN{FS=":"; print "# -*- mode: org -*-"; print "# please see gen-enclose-back-tick-list.sh"; print "# toggle todo C-c C-t"} $2>0 { print "* TODO " $1 "\t" $2}'

in Emacs

M-x grep-find

.. code-block:: text

   find ./ -not \( -path './howto/*' -o -path './technical/*' -o -path './RelNotes/*' \) -name "*.po" | xargs grep --color=auto -n -P '`(?:[^` ]+(?:[][<>]|\s)+){1,}[^` ]+`'

.. note::

   正規表現を \" (ダブルクォーテーション) で囲って数日ハマる。 \' (シングルクォーテーション) にしたら通った。

   grep の -e と -E (拡張正規表現)に注意。 -e だと \\+ だけど -E なら + で良い

正規表現を構築
--------------

Emacs の正規表現がサッパリ分からなかったので以下のようなスペニットを使って調べました。

scratch バッファで実行

.. code-block:: elisp

   (progn
     (setq re "`\\(\\(?:[^`\n ]+\\(?:[][<>]\\|\s\\)+\\)+[^`\n ]+\\)`")
     (setq text "便宜上、 `git blame --reverse START` は `git blame --reverse START..HEAD` と見なされます。")
     (list
       (string-match re text)
       (match-string 0 text)
       (match-beginning 0)
       (match-end 0)
       (string-match re text (match-end 0))
       (match-string 0 text)
       (match-beginning 0)
       (match-end 0)
       )
     )  ; c-j here in scratch-buffer

実行結果

.. code-block:: elisp

   (5 "`git blame --reverse START`" 5 32 35 "`git blame --reverse START..HEAD`" 35 68)

上記 elisp で記述した正規表現を M-x query-replace-regexp で
指定するときにはどのように書けばいいか知るには \*.po のバッファで M-: して ミニバッファーで eval します。
   
.. code-block:: elisp

   (query-replace-regexp "`\\(\\(?:[^`\n ]+\\(?:[][<>]\\|\s\\)+\\)+[^`\n ]+\\)`" "\+\\1\+")

.. comments...
   そして一度実行後、 今度は M-x query-replace-regexp すると、ヒストリに上記が出てきますので、
   それをどこかにメモっておいて下記コマンドで入力します。
   M-x query-replace-regexp

今回はコマンドにしてしまいました。

https://github.com/kuma35/elisp

の query-replace-enclosed-backtick-to-plus.el を使います。

作業
----

上記 gen-enclose-back-tick-list.sh で生成した enclose-back-tick-list.txt
に作業対象ファイルがリストされていますので、これを見ながらファイルそれぞれで以下の作業を行います。

バッファーがリードオンリーでは実行できないのでモードラインの U:%%- の最初の % をクリックして U:--- にする

M-x query-replace-enclosed-backtick-to-plus

実行後はモードラインの U:\*\*- をクリックして U:%\*- に戻す

.. warning::
   
   msgid も候補にあがってくるのでウッカリ置換しないよう注意。

   行をまたいだのは検出できないので、お手数ですが目検してください。

