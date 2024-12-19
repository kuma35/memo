.. -*- coding: utf-8; mode: rst; -*-

.. index:: gforth

制御フロー・スタック
====================

- 更新:2024年12月20日
- 作成:2024年7月27日

メリット
--------

知れば制御構造がらみで出てくるエラーの理解が進みます。 初見だと制御構造絡みのエラーは訳わからん

あー、 更にガンバレば独自の制御構造が作れます。
但し、 たいていの制御構造は既に用意されています…

解説
----

参照: (gforth) 6.9.6 Arbitrary control structures
https://kuma35.github.io/gforth-docs-ja/docs-ja-0/gforth/Arbitrary-control-structures.html

制御構造の「コンパイル時」に使用されるのが「制御フロー・スタック」(control-flow stack) です。

gforth の制御フロー・スタックは(定義ごとに)コンパイル時にデーター・スタック上に構築され使用されます。

逆に言うと、コンパイル時のみで制御構造の実行時にはの制御フロー・スタックは存在しません。

.. ::

   このバージョン(Gforth 0.7.9_20240418)のドキュメントの原文では3つのスタック項目を占めると書いてありましたが、
   ソースを拝むと 4 つだったので、訳文は 4 つに修正してあります。

スタック上に

( stack-state locals-list address type )

cs-roll, cs-pick, cs-drop は この4つ を 1組扱いして操作します。

- type ( defstart, live-orig, dead-orig, dest, do-dest, scopestart) ( TOS )

- address (of the branch or the instruction to be branched to) (second)

- locals-list (valid at address) (third)

- stack state address for checking (fourth)

6.9.6.1 Programming Style の例に cs-stack を例示してみましょう。
これは true IF の時は IF の ... を実行し、更に AGAINに到達しBEGINに戻り、
false IF のときは THEN に飛んでBEGIN〜AGAINループを抜ける事を意図しています。

.. code:: forth

   BEGIN ( cs: dest_BEGIN )
     \ ...
     IF ( cs: dest_BEGIN orig_IF )
       \ ...
   ( destを期待 ) AGAIN ( orig を期待) THEN

これをコンパイルすると以下のようにエラーとなります。
AGAINでは dest を期待しているからです。
   
.. code:: text

   : hoge  compiled
     BEGIN  compiled
       IF  compiled
     AGAIN THEN ; 
   *the terminal*:4:3: error: expected dest 
   >>>AGAIN<<< THEN ;

そこで、 IF の後ろで [ 1 CS-ROLL ] として順番を入れ替えます。
ここで注意してほしいのは「コンパイル時」の操作だということです。
制御構造にかかわらず前から後ろへ上から下へ順番にワードは処理されていきます。
IFブロックの中だからといって true IF の時だけ [ 1 CS-ROLL ] が処理される訳では無いということです。
IFが条件分岐として機能するのはあくまで実行時です。だんだん混乱してきましたね。訳者もよく混乱しています。

gforth (forth) は 自在変幻にコンパイルと実行が入り交じる(コンパイル自体もワードの「実行」によって行われる)
という実にわけわからんなんじゃこりゃなので、 そこらへん注意深くじっくり見つめましょう。


.. code:: forth

   BEGIN ( cs: dest_BEGIN )
     \ ...
     IF ( cs: dest_BEGIN orig_IF ) [ 1 CS-ROLL ] ( cs: orig_IF dest_BEGIN )
     \ ...
   ( destを期待 ) AGAIN ( orig を期待) THEN

.. code:: text
	  
   : hoge  compiled
     BEGIN  compiled
       IF [ 1 CS-ROLL ]  compiled
     AGAIN THEN ;  ok

正常にコンパイルできました。

ちょっとそれっぽく動くようにしてみましょう。

.. code:: forth

   : hoge  ( 0 ... n -- )
     BEGIN
       dup .
       IF [ 1 CS-ROLL ]
         ." true"
     AGAIN THEN ; 

.. code:: text

   0 1 2 3 4 5 hoge 5 true4 true3 true2 true1 true0  ok
   1 2 3 4 5 hoge 5 true4 true3 true2 true1 true
   *the terminal*:19:11: error: Stack underflow
   1 2 3 4 5 >>>hoge<<<

orig
----

前方参照のために cs-stack に積まれます。 orig を積む時、当該ワードは手元のアドレスに ダミーの ゼロ を書き込みます。
そのアドレスを orig の address にセットします。

orig を 消費するワードは、 その address の場所に、 orig を 消費するワードの位置のアドレス、 を書き込みます。

これで、 orig を積んだワードから、 orig を消費するワードへ、実行時にジャンプができるようになりました。

dest
----

dest を積むワードはその address を orig と同様にセットしますが、
そのアドレスの場所にはdest を積むワードのアクションへのアドレスをセットします。
これは後から書き換えはされません。

dest を消費するワードは dest から取り出した address を dest を消費するワードのジャンプ先としてセットします。

これで、 dest を消費するワードから dest を積んだワードへ実行時にジャンプできるようになりました。
