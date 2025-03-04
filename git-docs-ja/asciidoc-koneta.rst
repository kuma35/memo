.. -*- coding: utf-8; mode: rst; -*-

.. index:: asciidoc, git

Asciidoc 翻訳小ネタ集
=====================

- 作成:2025年1月9日

小ネタ集です。

バック・クォーテーション(back tick) \`hoge\`
--------------------------------------------

バック・クォーテーションで囲まれた文字列を含むフレーズ、たとえば

.. code-block:: text
   
   I am a `good` cook.

について、info では

.. code-block:: text
		
   I am a 'good' cook.

と、シングル・クォーテーションで囲って表示される。    

一方、 html では

.. code-block:: text

   I am a good cook.

と表記される。 html は

.. code-block:: html
		
   <p>I am a <code>good</code> cook.</p>

と <code> </code> で囲って表示されるため当該部分が monospace font 表示となり、
見た目には区別が付くようになっている。

では、 空白文字はどのようになるのだろうか

中身が空白のバッククォーテーション文字列 \` \`
..............................................

AsciiDoc で空白のみの `\` \`` (バック・クォーテーションで囲まれた文字列)があった場合は `' '` (シングル・クォーテーションで囲む)とか `" "` (ダブルクォーテーションマークで囲む)としなければならない。

なぜなら、 空白だけの `\` \`` 文字列の場合

.. code-block:: text

   delimiter is ` ` only.

といった場合、

info では

.. code-block:: text

   delimiter is ' ' only.

なのだけど、

html では

.. code-block:: html

   <p>delimiter is <code> </code> only.</p>

で、見かけは

.. code-block:: text

   delimiter is     only.

となってしまってなんことやらワケワカメになるのである。

よって、
AsciiDoc で空白のみの `\` \`` (バック・クォーテーションで囲まれた文字列)があった場合は `' '` (シングル・クォーテーションで囲む)とか `" "` (ダブルクォーテーションマークで囲む)としなければならない。

シングル・クォーテーション('hoge')
----------------------------------

シングル・クォーテーションで囲うと info では _hoge_ , html では 'hoge' 表記です。

ダブル・クォーテーション("hoge")
--------------------------------

ダブルクォーテーションマークで囲うと info , html とも "hoge" 表記です。


