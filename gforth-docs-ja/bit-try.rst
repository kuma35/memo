.. -*- coding: utf-8; mode: rst; -*-

.. index:: gforth

ちょいとお試しでGNU gforth Docker版を起動してみる
=================================================

- 更新:2024年12月20日

docker 版。 手元で試したのは Gforth 0.7.9_20241009 でした。

もってくる
----------

.. code:: bash
	  
   docker pull forthy42/gforth


起動
----

.. code:: bash

   $ docker run -ti --rm forthy42/gforth

終了
----

bye

画面
----

一番下のモードライン左側には現在のスタックの状態が表示されます(
TOS;スタック・トップから一部)
また、 .s でも非破壊的に見れます(同様にTOSから一部)

.. code-block:: 

   1 2 3 4 5 6 7 8 9 10 11 12 13 14 15 16 17 18 19 20 ok 20
   <20> #12 #13 #14 #15 #16 #17 #18 #19 #20

これは全体としては スタックに 20 個積まれていて、
そのうちの TOS 側 10 個だけが表示されています。
通常は一番右がTOSです。

そして | から右側は ワード(forthのコマンド) のライブラリカテゴリ(wordlist)検索順と、
これから定義されるワードが追加されるカテゴリ(wordlist)を示しています。
order でも同じ情報がでます。

スタッククリア
--------------

clearstacks

詳しい説明は
------------

オレオレ翻訳(gnu gforth 0.7.9_20240418)でスマンソ

https://kuma35.github.io/gforth-docs-ja/
