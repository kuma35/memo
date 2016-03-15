.. -*- coding: utf-8; mode: rst; -*-


mjpg-streamer
=============

.. code-block:: bash

   $ sudo apt-get install subversion libjpeg-dev imagemagick

最新版をsvnリポジトリから取得(今回は$HOMEに直接配置)。

.. code-block:: bash

   $ cd
   $ svn co https://svn.code.sf.net/p/mjpg-streamer/code/mjpg-streamer mjpg-streamer
   $ cd ~/mjpg-streamer
   $ make

.. note::

   このままの権限で使い続けるのでインストール(sudo make install)はしない。

.. code-block:: bash
   
   $ sudo gpasswd --add hideo video

ログアウト、ログインして反映する。

動作確認

.. code-block:: bash

   $ ./mjpg_streamer -i "./input_uvc.so -d /dev/video1 -y -r 1920x1080 -f 1" -o "./output_http.so -w www"

参考
....

http://www.hiramine.com/physicalcomputing/raspberrypi/webcamstreaming.html
