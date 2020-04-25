.. -*- coding: utf-8; mode: rst; -*-

.. index:: .bashrc

venv環境をプロンプトに表示
==========================

2020年04月11日

現在のvenv環境を知る
....................

activateで環境変数 VIRTUAL_ENV がセットされる。
deactivateでアンセット。

.. code-block:: bash

   $ echo $VIRTUAL_ENV
   /home/hideo/work/memosphinx/venv

よってこのまま表示するなら、
プロンプトで $VIRTUAL_ENV を指定するだけでOK。

venvフォルダを含むフォルダ名だけ表示したい。
............................................

.. code-block:: bash

   $ echo $(basename $(dirname $VIRTUAL_ENV))
   memosphinx

結論
....

.bashrcで以下の関数を定義。

.. code-block:: bash

   function venvname() {
       # https://qiita.com/ymdymd/items/51bf4145ec58654eaffc
       if [ "${VIRTUAL_ENV:+UNDEF}" ]; then
           echo $(basename $(dirname $VIRTUAL_ENV))
       fi
   }

プロンプトの方は以下のように書き換える。

.. code-block:: bash

   if [ "$color_prompt" = yes ]; then
       PS1='`venvname`\n${debian_chroot:+($debian_chroot)}\[\033[01;32m\]\u@\h\[\033[00m\]:\[\033[01;34m\]\w\[\033[00m\]\n\$ '
   else
       PS1='`venvname`\n${debian_chroot:+($debian_chroot)}\u@\h:\w\n\$ '
   fi

こうすると、venv環境内にあるときは以下のようなプロンプトとなる。

.. code-block:: bash

   (venv) memosphinx
   hideo@winston:~/work/memosphinx
   $
