.. -*- coding: utf-8; mode: rst; -*-

byobuのlsのdircolorsを変更
==========================

Ubuntu 16.04LTS(32bit)

byobu(backend screen)の中でlsしたときの色を変更する。

byoubの中でlsでディレクトリ表示するとダークブラウンの背景に灰色表示とか非常に見難いので、Ubuntuのデフォルトのdircolorsにする。

/usr/share/byobu/profiles/bashrc の中で、 /usr/share/byobu/profiles/dircolorsを読み込んでる。

全ユーザで共有する場合
----------------------

byobuを起動していない状態で

.. code-block:: bash

   dircolors -p > /tmp/dircolors.ubuntu
   cd /usr/share/byobu/profiles
   sudo cp /tmp/dircolors.ubuntu .
   sudo mv dircolors dircolors.byobu
   sudo ln -s dircolors.ubuntu dircolors


として無理やり変更。

自分の環境のみ
--------------

byobuを起動していない状態で

.. code-block:: bash

   dircolors -p > ${HOME}/.byobu/.dircolors

.byobu/promptファイル

.. code-block:: bash

   [ -r /usr/share/byobu/profiles/bashrc ] && . /usr/share/byobu/profiles/bashrc  #byobu-prompt#

行を追加する。

.. code-block:: bash

   [ -r /usr/share/byobu/profiles/bashrc ] && . /usr/share/byobu/profiles/bashrc  #byobu-prompt#
   if [ -x /usr/bin/dircolors ]; then
     test -r ${HOME}/.byobu/.dircolors && eval "$(dircolors -b ${HOME}~/.byobu/.dircolors)" || eval "$(dircolors -b)"
     alias ls='ls --color=auto'
     #alias dir='dir --color=auto'
     #alias vdir='vdir --color=auto'
     
     alias grep='grep --color=auto'
     alias fgrep='fgrep --color=auto'
     alias egrep='egrep --color=auto'
   fi
