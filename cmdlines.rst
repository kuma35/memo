.. -*- coding: utf-8; mode: rst; -*-

コマンドラインもろもろ
======================

2016年03月24日

カレントディレクトリ以下のファイルの大きい順に50個
--------------------------------------------------

.. code-block:: bash

   $ du -ch | sort -rh | head -n 50

特定のディレクトリ以下を除外したファイルリスト作成
--------------------------------------------------
   
.. code-block:: bash

   $ find $HOME \( -type d -and \( -name '.cache' -or -name '.compiz-1' \) -and -prune \) -or \( -type f -and -print \)   

参考
....

http://mollifier.hatenablog.com/entry/20090115/1231948700

pip install で追加された ~/.local/bin をパスに加える
----------------------------------------------------

~/.profile

.. code-block:: bash

   # for python pip install commands.
   if [ -d "$HOME/.local/bin" ]; then
       PATH="$HOME/.local/bin:$PATH"
   fi

.. code-block:: bash

   $ source ~/.profile

