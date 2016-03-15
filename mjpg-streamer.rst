.. -*- coding: utf-8; mode: rst; -*-


mjpg-streamer
=============

| sudo apt-get install subversion libjpeg-dev imagemagick

最新版をsvnリポジトリから取得(今回は$HOMEに直接配置)

| $ cd
| $ svn co https://svn.code.sf.net/p/mjpg-streamer/code/mjpg-streamer mjpg-streamer

make (installしない)

| $ cd ~/mjpg-streamer
| $ make

このままの権限で使い続けるのでインストール(sudo make install)はしない。


| $ sudo gpasswd --add <<username>> video

ログアウト、ログインして反映する

動作確認

| $ ./mjpg_streamer -i "./input_uvc.so -d /dev/video1 -y -r 1920x1080 -f 1" -o "./output_http.so -w www"


.. note::
   参考
   
   http://www.hiramine.com/physicalcomputing/raspberrypi/webcamstreaming.html
