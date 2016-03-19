.. -*- coding: utf-8; mode: rst; -*-

デバイス名を固定
================


高性能USB-TTL/485コンバータ(FT-UBF-TTL485)
------------------------------------------

高性能USB-TTL/485コンバータ(FT-UBF-TTL485) [#]_ は、
なにもしないと Arudino NANOと同様 /dev/ttyUSB%n で認識されるため、
どちらの認識が早いかで /dev/ttyUSB0 と /dev/ttyUSB1 が入れ替わることがあるので固定しておく。
更にFT232を使ったドングルを2種類繋ぐことがあるのでATTR{serial}で区別。

  | Bus 001 Device 010: ID 0403:6001 Future Technology Devices International, Ltd FT232 USB-Serial (UART) IC

ATTRS{serial}は以下のコマンドで調べた。

.. code-block:: bash

   $ udevadm info -q all -n /dev/ttyUSB0

/etc/udev/rules.d/ に 62-ft485r.rules を作成。

    # for USB-TTL/485 convertor FT-UBF-TTL485
    ATTRS{idVendor}=="0403",ATTRS{idProduct}=="6001",ATTRS{serial}=="A7039N11",KERNEL=="ttyUSB*",SYMLINK+="ttyFT485R"

参考
....

http://qiita.com/caad1229/items/309be550441515e185c0

24ピンDIP-ICサイズ FT232RL USB-シリアル変換モジュール
-----------------------------------------------------

USB-シリアル変換モジュール FT232RL [#]_

/etc/udev/rules.d/ に 62-ft232r.rules を作成。

    # for USB-TTL/232 convertor FT232RL
    ATTRS{idVendor}=="0403",ATTRS{idProduct}=="6001",ATTRS{serial}=="AH01JKEI",KERNEL=="ttyUSB*",SYMLINK+="ttyFT232R"

.. rubric:: Footnotes

.. [#] aitendo。1,250円(税別)( http://www.aitendo.com/product/10245 ) 2016年3月現在。
.. [#] 秋月電子 950円(税別)( http://akizukidenshi.com/catalog/g/gK-01977/ ) 2016年3月現在。

