.. -*- coding: utf-8; mode: rst; -*-

noip2 client daemon for sytemd
=============================

noip2 client インストール
-------------------------

| http://www.noip.com/download?page=linux

.. code-block:: bash

   $ wget http://www.no-ip.com/client/linux/noip-duc-linux.tar.gz
   $ tar xf noip-duc-linux.tar.gz
   $ cd noip-2.1.9-1/
   $ sudo make install

sign upのユーザ名とパスワードを聞いてくるので入力。配置場所はデフォルトでは
バイナリは /usr/local/bin/noip2 で、 conf は /usr/local/etc/noip2.conf

systemd対応
-----------

| https://bbs.archlinux.org/viewtopic.php?id=146167

を参考に /lib/systemd/system に noip2.service を作る。

.. code-block:: text

   [Unit]
   Description=No-IP Dynamic DNS Update Client
   After=network.target
   
   [Service]
   Type=forking
   ExecStart=/usr/bin/noip2
   
   [Install]
   WantedBy=multi-user.target

有効化する。

.. code-block:: bash

   $ sudo chmod 644 /usr/local/bin/noip2
   $ sudo systemctl enable noip2.service
   Created symlink from /etc/systemd/system/multi-user.target.wants/noip2.service to /lib/systemd/system/noip2.service.
   $ sudo systemctl status noip2.service
   ● noip2.service - No-IP Dynamic DNS Update Client
		Loaded: loaded (/lib/systemd/system/noip2.service; enabled; vendor preset: enabled)
		Active: inactive (dead)
   $ sudo systemctl start noip2.service
   $ sudo systemctl status noip2
   ● noip2.service - No-IP Dynamic DNS Update Client
		Loaded: loaded (/lib/systemd/system/noip2.service; enabled; vendor preset: enabled)
		Active: active (running) since 金 2017-01-27 04:55:15 JST; 21s ago
		Process: 1881 ExecStart=/usr/local/bin/noip2 (code=exited, status=0/SUCCESS)
   Main PID: 1883 (noip2)
   CGroup: /system.slice/noip2.service
           └─1883 /usr/local/bin/noip2
   
   1月 27 04:55:15 merit systemd[1]: Starting No-IP Dynamic DNS Update Client...
   1月 27 04:55:15 merit systemd[1]: Started No-IP Dynamic DNS Update Client.
   1月 27 04:55:15 merit noip2[1883]: v2.1.9 daemon started with NAT enabled
   1月 27 04:55:16 merit noip2[1883]: hogehoge.ddns.net was already set to XXX.XXX.XXX.XXX



