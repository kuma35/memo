.. -*- coding: utf-8; mode: rst; -*-

virtualenvからvenvへ移行
========================

2016年09月25日

Ubuntu 16.04LTS(Python 3.5.2)

準備
----

Ubuntu 16.04LTS(i386)ではpyvenvをインストールする必要があった。

.. code-block:: bash

   sudo apt install python3-venv

移行作業
--------

元の場所は ${HOME}/.virturalenvs/py3bottle として、移行先フォルダを ${HOME}/work/pyvenv/py3bottle とする。

まず、1階層上までフォルダを作る。

.. code-block:: bash

   mkdir -p ${HOME}/work/pyvenv

元々のremoteのurlを確認

.. code-block:: bash

   cd ${HOME}/.virturalenvs/py3bottle
   git remote -v

として表示されたoriginのURLを控えておく。
   
.. code-block:: bash

   cd ${HOME}/work/pyvenv
   git clone ${HOME}/.virtualenv/py3bottle

元のフォルダで git remote -v した結果をcloneしたリポジトリにも反映させる。

.. code-block:: bash

   git remote set-url origin <<url>>

当該フォルダのvenv環境作成

.. code-block:: bash
   
   pyvenv py3bottle

有効化
------

.. code-block:: bash

   source ${HOME}/work/pyvenv/py3bottle/bin/activate

とすると有効化できる。

以下のようにしてaliasを組んでおくと便利。

.. code-block:: bash

   alias py3bottle source ${HOME}/work/pyvenv/py3bottle/bin/activate;cd ${VIRTUAL_ENV}

py3bottleと打つと環境を有効化し、当該ディレクトリに移る。

.. code-block:: bash

   cd $VIRTUAL_ENV

当該ディレクトリに移動したい時。

venv無効化
----------

**有効化している状態で** deactivate と入力。

環境の削除
----------

virturalenvと異なりvenvの設定ファイルは当該フォルダ下にしかないので、
当該フォルダを削除するだけでよい。
