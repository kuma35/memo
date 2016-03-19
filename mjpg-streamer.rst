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

設定例
------

Webカメラ用サーバからport8080で送出し、apache2のリバースプロキシで /cam2 に見せる。

Webカメラ用サーバ(192.168.1.2)

.. code-block:: bash

   $ ./mjpg_streamer -i "./input_uvc.so -d /dev/video1 -r 1920x1080 -f 1 -y -n" -o "./output_http.so -p 8080 -w www"

Webサーバ(192.168.1.6) proxy.conf内で...
   
.. code-block:: apacheconf

   ProxyPass /cam2 http://192.168.1.2:808
   ProxyPassReverse /cam2 http://192.168.1.2:8080

   http://192.168.1.6/cam2 にアクセスすると mjpegで再生される。

デバイス名固定
--------------

mjpg-streamerはデバイス名へのシンボリックリンクを辿れないようなので、以下のようにrealpathコマンドを介して利用する。
なお、固定方法そのものについては :doc:`fix-devicename` を参照してください。

.. code-block:: bash

   $ ./mjpg_streamer -i "./input_uvc.so -d `realpath /dev/webcam2` -r 1920x1080 -f 1 -y -n" -o "./output_http.so -p 9999 -w www"		

参考
....

http://www.hiramine.com/physicalcomputing/raspberrypi/webcamstreaming.html
