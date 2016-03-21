.. -*- coding: utf-8; mode: rst; -*-

デバイス名を固定
================


高性能USB-TTL/485コンバータ(FT-UBF-TTL485)
------------------------------------------

高性能USB-TTL/485コンバータ(FT-UBF-TTL485) [#]_ は、
なにもしないと Arudino NANOと同様 /dev/ttyUSB%n で認識されるため、
どちらの認識が早いかで /dev/ttyUSB0 と /dev/ttyUSB1 が入れ替わることがあるので固定しておく。
更にFT232を使ったドングルを2種類繋ぐことがあるのでATTR{serial}で区別。

    Bus 001 Device 010: ID 0403:6001 Future Technology Devices International, Ltd FT232 USB-Serial (UART) IC

ATTRS{serial}は以下のコマンドで調べた。

.. code-block:: bash

   $ udevadm info -q all -n /dev/ttyUSB0

/etc/udev/rules.d/ に 62-ft485r.rules を作成。

    | # for USB-TTL/485 convertor FT-UBF-TTL485
    | ATTRS{idVendor}=="0403",ATTRS{idProduct}=="6001",ATTRS{serial}=="A7039N11",KERNEL=="ttyUSB*",SYMLINK+="ttyFT485R"

参考
....

http://qiita.com/caad1229/items/309be550441515e185c0

24ピンDIP-ICサイズ FT232RL USB-シリアル変換モジュール
-----------------------------------------------------

USB-シリアル変換モジュール FT232RL [#]_

/etc/udev/rules.d/ に 62-ft232r.rules を作成。

    | # for USB-TTL/232 convertor FT232RL
    | ATTRS{idVendor}=="0403",ATTRS{idProduct}=="6001",ATTRS{serial}=="AH01JKEI",KERNEL=="ttyUSB*",SYMLINK+="ttyFT232R"

ミニマイコンモジュール [N328P] [#]_
-----------------------------------

.. ブレッドボード上で使う事を想定したミニマイコンモジュール、Atmega328P/16MHzクリスタル/CH340G搭載、arduino NANOと互換、動作電源：5V

Arduino nano互換

    Bus 001 Device 019: ID 1a86:7523 QinHeng Electronics HL-340 USB-Serial adapter

/etc/udev/rules.d/ に 62-arduino-nano.rules を作成
    
    | # for Arduino nano compatible N328P
    | ATTRS{idVendor}=="1a86",ATTRS{idProduct}=="7523",KERNEL=="ttyUSB*",SYMLINK+="ttyN328P"

マイクロソフト ウェブカメラ LifeCam HD-5000
-------------------------------------------

mjpg-streamerではシンボリックリンクを認識しないので、識別用のみに使用し、realpathコマンドで得たパスを与える。

    Bus 002 Device 003: ID 045e:076d Microsoft Corp. LifeCam HD-5000

/etc/udev/rules.d/ に 63-ms-hd5000.rules を作成。

    | # for MS LifeCam HD-5000
    | ATTRS{idVendor}=="045e",ATTRS{idProduct}=="076d",KERNEL=="video*",SYMLINK+="webcam1"

マイクロソフト フルHD Webカメラ LifeCam Studio Q2F-00020
--------------------------------------------------------

mjpg-streamerではシンボリックリンクを認識しないので、識別用のみに使用し、realpathコマンドで得たパスを与える。

    Bus 001 Device 008: ID 045e:0772 Microsoft Corp. LifeCam Studio

/etc/udev/rules.d/ に 63-mslifecam.rules を作成。

    | # for MS LifeCam Studio Q2F-00020
    | ATTRS{idVendor}=="045e",ATTRS{idProduct}=="0772",KERNEL=="video*",SYMLINK+="webcam2"


USBロケットランチャー
---------------------

40-rocketlauncher.rules

    | SUBSYSTEM=="usb", ENV{DEVTYPE}=="usb_device", ACTION=="add", SYSFS{idVendor}=="1941", SYSFS{idProduct}=="8021", GROUP="plugdev", MODE="0660"
    | SUBSYSTEM=="usb", ENV{DEVTYPE}=="usb_device", ACTION=="add", SYSFS{idVendor}=="0a81", SYSFS{idProduct}=="0701", GROUP="plugdev", MODE="0660"
    | SUBSYSTEM=="usb", ENV{DEVTYPE}=="usb_device", ACTION=="add", SYSFS{idVendor}=="1130", SYSFS{idProduct}=="0202", GROUP="plugdev", MODE="0660"


.. rubric:: Footnotes

.. [#] aitendo。1,250円(税別)( http://www.aitendo.com/product/10245 ) 2016年3月現在。
.. [#] 秋月電子 950円(税別)( http://akizukidenshi.com/catalog/g/gK-01977/ ) 2016年3月現在。
.. [#] aitendo。1,250円(税別)( http://www.aitendo.com/product/10700 ) 2016年3月現在。
