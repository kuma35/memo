.. -*- coding: utf-8; mode: rst; -*-

x11vnc
======

Ubuntu15.10MATE(on Raspberry Pi 2 model B)

参考のとおりに。

.. code-block:: bash

   $ sudo apt install x11vnc
   $ sudo x11vnc –storepasswd /etc/x11vnc.pass

/lib/systemd/system/x11vnc.service

.. code-block:: none

   [Unit]
   Description=Start x11vnc at startup.
   After=multi-user.target
   
   [Service]
   Type=simple
   ExecStart=/usr/bin/x11vnc -auth guess -forever -loop -noxdamage -repeat -rfbauth /etc/x11vnc.pass -rfbport 5900 -shared
   
   [Install]
   WantedBy=multi-user.target

.. code-block:: bash

   $ sudo systemctl daemon-reload
   $ sudo systemctl enable x11vnc.service

ポート5900を開放する。

.. code-block:: bash

   $ sudo ufw allow 5900

マシンをリブート

.. code-block:: bash

   $ sudo reboot

クライアントマシンからアクセス。

.. code-block:: bash

   $ sudo gvncviewer 192.168.1.2:0

Nexus7では「アンドロイドvnc」で接続できた。Vnc Viewerでは繋がらなかったが原因は未調査。

参考
....

http://c-nergy.be/blog/?p=8361
