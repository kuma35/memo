.. -*- coding: utf-8; mode: rst; -*-

arduino-mk with Arduino IDE 1.6.12
==================================

環境
----

Python 3.5.2(Ubuntu 16.04LTS(i386))

pyserial==3.1.1

Install arduino-mk 
------------------

とってくる。

.. code-block:: bash
   
   git clone https://github.com/sudar/Arduino-Makefile.git

ここでは ${HOME}/work/Arduino-Makefile に置いたとする。

Get Arduino IDE 1.6.12
----------------------

ArduinoのページからLinux32用を取得し展開。

ここでは ${HOME}/.arduino/arduino-1.6.12 とする。

Makefile
--------

.. code-block:: make

   # Arduino Make file. Refer to https://github.com/sudar/Arduino-Makefile
   ARDUINO_DIR = $(HOME)/.arduino/arduino-1.6.12
   AVR_TOOLS_DIR = $(ARDUINO_DIR)/hardware/tools/avr
   ARDMK_DIR = $(HOME)/work/Arduino-Makefile
   BOARD_TAG = uno
   MONTOR_PORT = /dev/ttyACM0
   AVRDUDE_CONF = $(ARDUINO_DIR)/hardware/tools/avr/etc/avrdude.conf
   include $(HOME)/work/Arduino-Makefile/Arduino.mk

Arduinoのライブラリ指定する場合は$(HOME)/sketchbook/librariesに追加の上、
ARDUINO_LIBSに列記してください。   

.. code-block:: make

   # Arduino Make file. Refer to https://github.com/sudar/Arduino-Makefile
   ARDUINO_DIR = $(HOME)/.arduino/arduino-1.6.12
   AVR_TOOLS_DIR = $(ARDUINO_DIR)/hardware/tools/avr
   ARDMK_DIR = $(HOME)/work/Arduino-Makefile
   BOARD_TAG = uno
   MONTOR_PORT = /dev/ttyACM0
   AVRDUDE_CONF = $(ARDUINO_DIR)/hardware/tools/avr/etc/avrdude.conf
   ARDUINO_LIBS += Wire Adafruit_MotorShield
   include $(HOME)/work/Arduino-Makefile/Arduino.mk

make
----

compileのみ。

.. code-block:: bash

   make

compileとアップロード。
   
.. code-block:: bash
		   
   make upload

その他
------

シリアルコンソールはino serialの方が
使いやすかったのでそっちを使っています。
