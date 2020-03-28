.. -*- coding: utf-8; mode: rst; -*-

.. N10JCは主にサーバとして使うので設定した。
   以後は基本ssh経由でアクセス。Xも起動せずに使う。      


SSHサーバを建てる
=================

2018年08月19日

サーバにOpenSSHをインストール。

.. code-block:: bash

   $ sudo apt-get install openssh-server

クライアントで共通鍵、秘密鍵生成。ここではclientのマシン名をmeviusとする。
既に作成済のものがあればそのまま使う。
   
.. code-block:: bash

   $ ssh-keygen -t rsa -f ~/.ssh/id_rsa.mevius

.ssh に id_rsa.mevius と id_rsa.mevius.pub が出来る。
   
公開鍵をサーバのauthorized_keyに追加。

.. code-block:: bash

   $ mkdir ~/.ssh
   $ cd .ssh
   $ cat id_rsa.mevius.pub >> authorized_keys

クライアントでは秘密鍵を指定してsshにアクセス。
.ssh/configに書いておくと便利。

.. code-block:: bash

   Host 192.168.1.1
   IdentityFile    ~/.ssh/id_rsa.mevius
   User            hideo

.. code-block:: bash
	
   $ ssh 192.168.1.1

とすると秘密鍵~/.ssh/id_rsa.meviusを自動的に使う。

以後はアクセスするサーバが増える度に手元のid_rsa.mevius.pubをサーバに転送し、authorized_keyへ追加する。

