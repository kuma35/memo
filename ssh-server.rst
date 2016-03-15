.. -*- coding: utf-8; mode: rst; -*-

.. N10JCは主にサーバとして使うので設定した。
   以後は基本ssh経由でアクセス。Xも起動せずに使う。      


SSHサーバを建てる
=================

.. code-block:: bash

   $ sudo apt-get install openssh-server
   $ ssh-keygen -t rsa
   $ cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys

秘密鍵をクライアントに渡す

クライアントでは秘密鍵を指定してsshにアクセス。
.ssh/configに書いておくと便利。

	| Host 192.168.1.1
	|         IdentityFile    ~/.ssh/id_rsa.hoge
	|         User            hideo

.. code-block:: bash
	
   $ ssh 192.168.1.1

とすると秘密鍵~/.ssh/id_rsa.hogeを自動的に使う。
