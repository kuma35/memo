.. -*- coding: utf-8; mode: rst; -*-

.. index:: mjpg-streamer; 2020ver.

mjpg-streamer
=============

2020年03月28日

インストール
------------

.. code-block:: bash
		
   $ sudo apt install cmake libv4l-dev libjpeg-dev imagemagick
   $ cd ~/work
   $ git clone https://github.com/jacksonliam/mjpg-streamer.git
   $ cd mjpg-streamer/mjpg-streamer-experimental
   $ make
   $ sudo make install

.. note::

   systemd対応するので sudo make install まで実施する。

mjpg-streamer用htmlファイルの場所を旧環境に合わせる。
   
.. code-block:: bash
		
   $ sudo cp -rp www /home/httpd/mjpg-streamer

.. note::

   動作確認はデバイス名固定、デバイス接続後に行います。

.. index:: udev
   
デバイス名固定
--------------

参考: udev - ArchWiki ( https://wiki.archlinux.jp/index.php/Udev )

調べる
......

.. code-block:: bash
   
   $ sudo udevadm info -q path -n /dev/webcam2
   /devices/platform/soc/3f980000.usb/usb1/1-1/1-1.5/1-1.5:1.0/video4linux/video0

で、得られたデバイスパスを以下で指定する。

.. code-block:: bash
   
   sudo udevadm info -a -p /devices/platform/soc/3f980000.usb/usb1/1-1/1-1.5/1-1.5:1.0/video4linux/video0

.. code-block:: text
   :caption: 実行結果(抜粋)

   Udevadm info starts with the device specified by the devpath and then
   walks up the chain of parent devices. It prints for every device
   found, all possible attributes in the udev rules key format.
   A rule to match, can be composed by the attributes of the device
   and the attributes from one single parent device.
   
   looking at device '/devices/platform/soc/3f980000.usb/usb1/1-1/1-1.5/1-1.5:1.0/video4linux/video0':
     KERNEL=="video0"
     SUBSYSTEM=="video4linux"
     DRIVER==""
     ATTR{dev_debug}=="0"
     ATTR{index}=="0"
   
   looking at parent device '/devices/platform/soc/3f980000.usb/usb1/1-1/1-1.5/1-1.5:1.0':
     KERNELS=="1-1.5:1.0"
     SUBSYSTEMS=="usb"
     DRIVERS=="uvcvideo"
     ATTRS{authorized}=="1"
     ATTRS{bAlternateSetting}==" 0"
     ATTRS{bInterfaceClass}=="0e"
     ATTRS{bInterfaceNumber}=="00"
     ATTRS{bInterfaceProtocol}=="00"
     ATTRS{bInterfaceSubClass}=="01"
     ATTRS{bNumEndpoints}=="01"
     ATTRS{iad_bFirstInterface}=="00"
     ATTRS{iad_bFunctionClass}=="0e"
     ATTRS{iad_bFunctionProtocol}=="00"
     ATTRS{iad_bFunctionSubClass}=="03"
     ATTRS{iad_bInterfaceCount}=="02"
     ATTRS{supports_autosuspend}=="1"
   
   looking at parent device '/devices/platform/soc/3f980000.usb/usb1/1-1/1-1.5':
     KERNELS=="1-1.5"
     SUBSYSTEMS=="usb"
     DRIVERS=="usb"
     ATTRS{authorized}=="1"
     ATTRS{avoid_reset_quirk}=="0"
     ATTRS{bConfigurationValue}=="1"
     ATTRS{bDeviceClass}=="ef"
     ATTRS{bDeviceProtocol}=="01"
     ATTRS{bDeviceSubClass}=="02"
     ATTRS{bMaxPacketSize0}=="64"
     ATTRS{bMaxPower}=="500mA"
     ATTRS{bNumConfigurations}=="1"
     ATTRS{bNumInterfaces}==" 5"
     ATTRS{bcdDevice}=="0113"
     ATTRS{bmAttributes}=="80"
     ATTRS{busnum}=="1"
     ATTRS{configuration}==""
     ATTRS{devnum}=="4"
     ATTRS{devpath}=="1.5"
     ATTRS{devspec}=="(null)"
     ATTRS{idProduct}=="0772"
     ATTRS{idVendor}=="045e"
     ATTRS{ltm_capable}=="no"
     ATTRS{manufacturer}=="Microsoft"
     ATTRS{maxchild}=="0"
     ATTRS{quirks}=="0x0"
     ATTRS{removable}=="removable"
     ATTRS{rx_lanes}=="1"
     ATTRS{speed}=="480"
     ATTRS{tx_lanes}=="1"
     ATTRS{urbnum}=="5183816"
     ATTRS{version}==" 2.00"


マイクロソフト ウェブカメラ LifeCam HD-5000
...........................................

lsusb コマンドでも調査
    Bus 002 Device 003: ID 045e:076d Microsoft Corp. LifeCam HD-5000

/etc/udev/rules.d/ に 63-ms-hd5000.rules を作成。

.. code-block:: bash
   :caption: /etc/udev/rules.d/63-ms-hd5000.rules

   # for MS LifeCam HD-5000
   ATTRS{idVendor}=="045e",ATTRS{idProduct}=="076d",KERNEL=="video*",SYMLINK+="webcam1"

マイクロソフト フルHD Webカメラ LifeCam Studio Q2F-00020
........................................................

lsusb コマンドでも調査
    Bus 001 Device 008: ID 045e:0772 Microsoft Corp. LifeCam Studio

/etc/udev/rules.d/ に 63-mslifecam.rules を作成。

.. code-block:: bash
   :caption: /etc/udev/rules.d/63-mslifecam.rules

   # for MS LifeCam Studio Q2F-00020
   ATTRS{idVendor}=="045e",ATTRS{idProduct}=="0772",KERNEL=="video*",SYMLINK+="webcam2"

動作確認
--------

.. note::

   利用するユーザはvideoグループに属している必要があるので追加する。

   .. code-block:: bash

      sudo gpasswd --add <<username>> video

   一旦ログアウト、ログインして反映する。

上記デバイス MS LifeCam HD-5000 の場合(ハーフHD) ※下記例ではポート9999でリッスン。

.. code-block:: bash

   $ /usr/local/bin/mjpg_streamer -i "input_uvc.so -d /dev/webcam1 -y -r 1280x720 -f 1" -o "output_http.so -p 9999 -w /home/httpd/mjpg-streamer"

上記デバイス MS LifeCam Studio Q2F-00020 の場合(フルHD) ※下記例ではポート9998でリッスン。

.. code-block:: bash

   $ /usr/local/bin/mjpg_streamer -i "input_uvc.so -d /dev/webcam2 -y -r 1920x1080 -f 1" -o "output_http.so -p 9998 -w /home/httpd/mjpg-streamer"

.. note::

   2016年時点ではデバイスのシンボリックリンクを辿れなくて
   'realpath /dev/webcam1' などとする必要があったが、
   2020年時点では不要になっている。

Webブラウザでひらけば拝めるはず…

* webcam1 http://192.168.999.999:9999
* webcam2 http://192.168.999.999:9998

CTRL+Cで終了。

.. index:: systemd

systemd対応
-----------

両カメラとも上記デバイス名固定が動作しているとして、

webcam2
.......

.. code-block:: ini
   :caption: /home/httpd/django/webcam2.service
   
   $ cat /home/httpd/django/webcam2.service
   # MJPG-Streamer with MS LifeCam Studio Q2F-00020 on /dev/webcam2
   # please add TAG+="systemd" in udev-rule for /dev/webcam2
   
   [Unit]
   Description=mjpg_streamer with MS LifeCam Studio Q2F-00020 on /dev/webcam2
   After=udev.target
   # After=sound.target
   After=syslog.target
   BindsTo=/dev/webcam2
   After=/dev/webcam2
   
   [Service]
   type=simple
   ExecStart=/usr/local/bin/mjpg_streamer -i 'input_uvc.so -d /dev/webcam2 -y -r 1920x1080 -f 1' -o 'output_http.so -p 9998 -www /home/httpd/mjpg-streamer'
   
   [Install]
   WantedBy=multi-user.target
   WantedBy=/dev/webcam2



systemd対応(USBポート指定版)
----------------------------

ググると以下の例がゴロゴロ出てくる。でもこの指定だと
挿しているUSBハブやUSBポートまで指定しちゃってるんである。

当然、うっかり挿すポートを変更すると動かないのである。

これが役にたつというと、同じ商品を複数運用するなどの場合かしらん。
挿すUSBポートによって区別するとか。もちろん、そのポートが故障したら
この設定ファイルも修正しなければならない。


webcam1
.......

当該デバイスを調べる。挿すポートによって違うので必ず調べること。当然運用中もこのポートに固定すること。
下記はRaspberry Pi 3 USBポート右下(HDMIポートを左に見て)

.. code-block:: bash

   $ sudo systemctl list-units -t device | grep "LifeCam HD-5000"
   sys-devices-platform-soc-3f980000.usb-usb1-1\x2d1-1\x2d1.5-1\x2d1.5:1.2-sound-card0.device   loaded active plugged LifeCam HD-5000

webcam2
.......

当該デバイスを調べる。挿すポートによって違うので必ず調べること。当然運用中もこのポートに固定すること。
下記はRaspberry Pi 3 USBポート左下(HDMIポートを左に見て)

.. code-block:: bash

   $ sudo systemctl list-units -t device | grep "LifeCam Studio"
   sys-devices-platform-soc-3f980000.usb-usb1-1\x2d1-1\x2d1.3-1\x2d1.3:1.2-sound-card1.device   loaded active plugged LifeCam Studio

.. code-block:: ini
   :caption: /home/httpd/django/webcam2.service
   
   [Unit]
   Description=MJPG-Streamer with MS LifeCam Studio Q2F-00020
   After=udev.target
   After=sound.target
   After=syslog.target
   
   BindsTo=sys-devices-platform-soc-3f980000.usb-usb1-1\x2d1-1\x2d1.3-1\x2d1.3:1.2-sound-card1.device
   After=sys-devices-platform-soc-3f980000.usb-usb1-1\x2d1-1\x2d1.3-1\x2d1.3:1.2-sound-card1.device
   
   [Service]
   type=simple
   ExecStart=/usr/local/bin/mjpg_streamer -i 'input_uvc.so -d /dev/webcam2 -y -r 1920x1080 -f 1' -o 'output_http.so -p 9998 -www /home/httpd/mjpg-streamer'
   
   [Install]
   WantedBy=multi-user.target
   WantedBy=sys-devices-platform-soc-3f980000.usb-usb1-1\x2d1-1\x2d1.3-1\x2d1.3:1.2-sound-card1.device

インストール

.. code-block:: bash

   cd /etc/systemd/system
   sudo ln -s /home/httpd/django/webcam2.service

動作確認

.. code-block:: bash

   sudo systemctl start webcam2
   sudo systemctl status webcam2

Webブラウザで http://192.168.999.999:9998 を拝んで動作確認する。

起動時に起動するようにする(しばしば忘れる)

.. code-block:: bash

   sudo systemctl enable webcam2

.. index:: lighttpd

..
  設定例
  httpサーバのリバースプロキシで /stream とか /snapshot で見せる。
