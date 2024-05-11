.. -*- coding: utf-8; mode: rst; -*-

.. index:: kivy; kivy.resources;

kivy.resources
==============

2023年03月16日

kivy.resources.resouce_paths
----------------------------

「あらかじめ決められた一連のフォルダーを検索します。 」

っていうのがこのリストである。

手元で見た値は以下のようになっている(<venv_dir>は /home/hoge/fuga/venv 等)

.. code-block:: python

   ['.',
   '',
   '<venv_dir>/lib/python3.10/site-packages/kivy',
   '<venv_dir>/lib/python3.10/site-packages/kivy/data/..']

で、kivyのソース・コードは

.. code-block:: python

   ['.',
   dirname(sys.argv[0]),
   dirname(kivy.__file__),
   join(kivy_data_dir, '..')]

iOS の場合のソースコードは
   
.. code-block:: python

   ['.',
   dirname(sys.argv[0]),
   join(dirname(sys.argv[0]), 'YourApp'),
   dirname(kivy.__file__),
   join(kivy_data_dir, '..')]

である。

ただし、検索時は `reversed(resource_paths)` するので後ろから前への順番検索されることに注意。

resource_add_path
-----------------

resources_paths の「末尾」に追加する。よって追加した path が検索対象として
真っ先に検索される。

既に登録済のものがあった場合は何もしないので、
優先順位を上げたい場合は
一旦 resource_remove_path してから resource_add_path する必要があるだろう。

追加する path は一切加工されない。 abspath() とか一切しない。

resource_remove_path
--------------------

既にリストの中に path があれば削除する。

引数で指定する path は一切加工しないので、リスト中に存在するパスと全く
同じものを指定しなければならない。

resource_find
-------------

filename に resource_paths というリストの中の
パスを「逆順」に適用しながら、探します。

つまり resource_paths に /hoge/fuga というパスが
あれば join('/hoge/fuga', filename) というファイルを探します。

無かったら作成とかしません。無かったら `None` を返します。
まずそこが大前提です。

よって filename が None だったら 単に return します。

.. comment

   Python 3.11.2 Documentation
   Python チュートリアル » 4. その他の制御フローツール
   4.7. 関数を定義する より
   return 文では、関数から一つ値を返します。
   return の引数となる式がない場合、 None が返ります。
   関数が終了したときにも None が返ります。

#. use_cache が True の場合は 60 秒以内は無条件にキャッシュされた
   ものが返ってきてしまいます。
   一旦 use_chche を False にして使うか、 60 秒を超えて待ちましょう(?)
#. filename の 先頭が 'atlas://' なら
   何もせずに filename をそのまま返します。
#. abspath(filename) 存在してるパスなら abspath() したものを返します。
#. abspath(join(<resource_paths のパス>, filename)) が
   存在したならそれを返します。
#. filename.startswith("data:") filename が 'data:' で始まる場合は
   filenameそのものを返します
#. use_cache が True で、 返す found_filename があるなら、
   found_filename をキャッシュに登録します。
#. とにかく何かあれば found_filename を返して 無ければ None を返します。
   


