.. -*- coding: utf-8; mode: rst; -*-

ufw
===

.. code-block:: bash

   $ sudo ufw default DENY
   $ sudo ufw allow OpenSSH
   $ sudo ufw enable

/etc/ufw/applications.d/openssh-outer-server

.. code-block:: bash

   [OpenSSH outer]
   title=Secure shell server, an rshd replacement
   description=OpenSSH is a free implementation of the Secure Shell protocol.
   ports=10000/tcp

ルールを削除する方法
--------------------

その1.ルールの追加したときのコマンドラインでufwの直後にdeleteを付けて実行

追加時

.. code-block:: bash

   $ sudo ufw allow 8080

それを削除する。

.. code-block:: bash

   $ sudo ufw delete allow 8080

その2.行番号を指定して削除

.. code-block:: bash
   
   $ sudo ufw status numbered

.. code-block:: none

   状態: アクティブ
    
        To                         Action      From
        --                         ------      ----
   [ 1] 9999/tcp                   ALLOW IN    Anywhere
   [ 2] 80                         ALLOW IN    Anywhere
   [ 3] OpenSSH                    ALLOW IN    Anywhere
   [ 4] Samba                      ALLOW IN    Anywhere
   [ 5] 8080                       ALLOW IN    Anywhere
   [ 6] 9999/tcp (v6)              ALLOW IN    Anywhere (v6)
   [ 7] 80 (v6)                    ALLOW IN    Anywhere (v6)
   [ 8] OpenSSH (v6)               ALLOW IN    Anywhere (v6)
   [ 9] Samba (v6)                 ALLOW IN    Anywhere (v6)
   [10] 8080 (v6)                  ALLOW IN    Anywhere (v6)

.. code-block:: bash

   $ sudo ufw delete 5

.. code-block:: none
		   
   削除:
   allow 8080
   操作を続けますか (y|n)? y
   ルールを削除しました

.. code-block:: bash

   $ sudo ufw delete 10

途中に追加する
--------------

insert <<number>> RULE

<<number>> の位置にRULEを挿入する。元々<<number>>の位置にあったRULEは後ろにズレる。
