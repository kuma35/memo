.. -*- coding: utf-8; mode: rst; -*-


サービス自動起動無効
====================

2016年03月16日

.. code-block:: bash

   $ sudo systemctl | grep cups
   cups.path            loaded active running   CUPS Scheduler
   cups-browsed.service loaded active running   Make remote CUPS printers available locally
   cups.service         loaded active running   CUPS Scheduler
   cups.socket          loaded active running   CUPS Scheduler
   
   $ sudo systemctl disable cups.path cups-browsed.service cups.service cups.socket
