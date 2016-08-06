.. -*- coding: utf-8; mode: rst; -*-

virtualenvからvenvへ移行
========================

Ubuntu 16.04LTS(Python 3.5.2)

準備
----

Ubuntu 16.04LTS(i386)ではpyvenvをインストールする必要があった。

.. code-block:: bash

   sudo apt install python3-venv

セットアップ
------------

#. 使うフォルダを決める。ここでは ${HOME}/work/pyenv/py3bottle とする。
#. cd ${HOME}/work/pyenv
#. git clone ${HOME}/.virtualenv/py3bottle py3bottle
#. pyenv py3bottle

有効化
------

.. code-block:: bash

   source ${HOME}/work/pyenv/py3bottle/bin/activate

とすると有効化できる。

以下のようにしてaliasを組んでおくと便利。

.. code-block:: bash

   alias py3bottle source ${HOME}/work/pyvenv/py3bottle/bin/activate;cd ${HOME}/work/pyvenv/py3bottle

py3bottleと打つと環境を有効化し、当該ディレクトリに移る。

無効化
------

**有効化している状態で** deactivate と入力。

環境の削除
----------

virturalenvと異なりvenvの設定ファイルは当該フォルダ下にしかないので、
当該フォルダを削除するだけでよい。
