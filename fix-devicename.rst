.. -*- coding: utf-8; mode: rst; -*-

デバイス名を固定
================


高性能USB-TTL/485コンバータ(FT-UBF-TTL485)
------------------------------------------

高性能USB-TTL/485コンバータ(FT-UBF-TTL485) [#]_ は、
なにもしないと Arudino NANOと同様 /dev/ttyUSB%n で認識されるため、
どちらの認識が早いかで /dev/ttyUSB0 と /dev/ttyUSB1 が入れ替わることがあるので固定しておく。

   Bus 001 Device 010: ID 0403:6001 Future Technology Devices International, Ltd FT232 USB-Serial (UART) IC

/etc/udev/rules.d/ に 62-ft232.rules を作成

.. code-block:: bash

   # for USB-TTL/485 convertor FT-UBF-TTL485
   ATTRS{idVendor}=="0403",ATTRS{idProduct}=="6001",KERNEL=="ttyUSB*",SYMLINK+="ttyFDTI"

参考
....

http://qiita.com/caad1229/items/309be550441515e185c0

.. rubric:: Footnotes

.. [#] aitendo。1,250円(税別)( http://www.aitendo.com/product/10245 ) 2016年3月現在。

