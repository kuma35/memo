.. -*- coding: utf-8; mode: rst; -*-

================
デバイス名を固定
================


高性能USB-TTL/485コンバータ [FT-UBF-TTL485]
http://www.aitendo.com/product/10245

と


ミニマイコンモジュール [N328P](Arudino Nano 328 5V 互換)
http://www.aitendo.com/product/10700
のデバイス名を固定してみる

なにもしないと 両方共 /dev/ttyUSB%n で認識されるため、どちらの認識が早いかで /dev/ttyUSB0 と /dev/ttyUSB1 が入れ替わることがある。

FT-UBF-TTL485

| Bus 001 Device 010: ID 0403:6001 Future Technology Devices International, Ltd FT232 USB-Serial (UART) IC

/etc/udev/rules.d/ に 62-ft232.rules を作成

| # for USB-TTL/485 convertor FT-UBF-TTL485
| ATTRS{idVendor}=="0403",ATTRS{idProduct}=="6001",KERNEL=="ttyUSB*",SYMLINK+="ttyFDTI"


N328P

| Bus 001 Device 011: ID 1a86:7523 QinHeng Electronics HL-340 USB-Serial adapter

| /dev/ttyNANO%n にしてみる

Microsoft Lifecam Studio

| # for Microsoft LifeCam Studio Q2F-00020
| SUBSYSTEM=="usb", ATTR{idVendor}=="045e", ATTR{idProduct}=="0772", GROUP="video" SYMLINK+="webcam2"

当該デバイスを抜き差し

| lsusb -s 001:010 -v
| lsusb -s 001:011 -v

| $ udevadm -q all -n /dev/ttyUSB0
| P: /devices/pci0000:00/0000:00:1d.7/usb1/1-3/1-3.4/1-3.4:1.0/ttyUSB0/tty/ttyUSB0
| N: ttyUSB0
| S: serial/by-id/pci-FTDI_FT232R_USB_UART_A7039N11-if00-port0
| S: serial/by-path/pci-0000:00:1d.7-usb-0:3.4:1.0-port0
| E: DEVLINKS=/dev/serial/by-id/pci-FTDI_FT232R_USB_UART_A7039N11-if00-port0 /dev/serial/by-path/pci-0000:00:1d.7-usb-0:3.4:1.0-port0
| E: DEVNAME=/dev/ttyUSB0
| E: DEVPATH=/devices/pci0000:00/0000:00:1d.7/usb1/1-3/1-3.4/1-3.4:1.0/ttyUSB0/tty/ttyUSB0
| E: ID_BUS=pci
| E: ID_MM_CANDIDATE=1
| E: ID_MODEL=FT232R_USB_UART
| E: ID_MODEL_ENC=FT232R\x20USB\x20UART
| E: ID_MODEL_FROM_DATABASE=FT232 USB-Serial (UART) IC
| E: ID_MODEL_ID=0x27cc
| E: ID_PATH=pci-0000:00:1d.7-usb-0:3.4:1.0
| E: ID_PATH_TAG=pci-0000_00_1d_7-usb-0_3_4_1_0
| E: ID_PCI_CLASS_FROM_DATABASE=Serial bus controller
| E: ID_PCI_INTERFACE_FROM_DATABASE=EHCI
| E: ID_PCI_SUBCLASS_FROM_DATABASE=USB controller
| E: ID_REVISION=0600
| E: ID_SERIAL=FTDI_FT232R_USB_UART_A7039N11
| E: ID_SERIAL_SHORT=A7039N11
| E: ID_TYPE=generic
| E: ID_USB_DRIVER=ftdi_sio
| E: ID_USB_INTERFACES=:ffffff:
| E: ID_USB_INTERFACE_NUM=00
| E: ID_VENDOR=FTDI
| E: ID_VENDOR_ENC=FTDI
| E: ID_VENDOR_FROM_DATABASE=Future Technology Devices International, Ltd
| E: ID_VENDOR_ID=0x8086
| E: MAJOR=188
| E: MINOR=0
| E: SUBSYSTEM=tty
| E: TAGS=:systemd:
| E: USEC_INITIALIZED=578748185

| $ udevadm -q all -n /dev/ttyUSB1
| P: /devices/pci0000:00/0000:00:1d.7/usb1/1-3/1-3.2/1-3.2:1.0/ttyUSB1/tty/ttyUSB1
| N: ttyUSB1
| S: serial/by-id/pci-1a86_USB2.0-Serial-if00-port0
| S: serial/by-path/pci-0000:00:1d.7-usb-0:3.2:1.0-port0
| E: DEVLINKS=/dev/serial/by-path/pci-0000:00:1d.7-usb-0:3.2:1.0-port0 /dev/serial/by-id/pci-1a86_USB2.0-Serial-if00-port0
| E: DEVNAME=/dev/ttyUSB1
| E: DEVPATH=/devices/pci0000:00/0000:00:1d.7/usb1/1-3/1-3.2/1-3.2:1.0/ttyUSB1/tty/ttyUSB1
| E: ID_BUS=pci
| E: ID_MM_CANDIDATE=1
| E: ID_MODEL=USB2.0-Serial
| E: ID_MODEL_ENC=USB2.0-Serial
| E: ID_MODEL_FROM_DATABASE=HL-340 USB-Serial adapter
| E: ID_MODEL_ID=0x27cc
| E: ID_PATH=pci-0000:00:1d.7-usb-0:3.2:1.0
| E: ID_PATH_TAG=pci-0000_00_1d_7-usb-0_3_2_1_0
| E: ID_PCI_CLASS_FROM_DATABASE=Serial bus controller
| E: ID_PCI_INTERFACE_FROM_DATABASE=EHCI
| E: ID_PCI_SUBCLASS_FROM_DATABASE=USB controller
| E: ID_REVISION=0254
| E: ID_SERIAL=1a86_USB2.0-Serial
| E: ID_TYPE=generic
| E: ID_USB_CLASS_FROM_DATABASE=Vendor Specific Class
| E: ID_USB_DRIVER=ch341
| E: ID_USB_INTERFACES=:ff0102:
| E: ID_USB_INTERFACE_NUM=00
| E: ID_VENDOR=1a86
| E: ID_VENDOR_ENC=1a86
| E: ID_VENDOR_FROM_DATABASE=QinHeng Electronics
| E: ID_VENDOR_ID=0x8086
| E: MAJOR=188
| E: MINOR=1
| E: SUBSYSTEM=tty
| E: TAGS=:systemd:
| E: USEC_INITIALIZED=685787095

  
| $ udevadm info -a -p $(udevadm info -q path -n /dev/ttyUSB0)
| 
| Udevadm info starts with the device specified by the devpath and then
| walks up the chain of parent devices. It prints for every device
| found, all possible attributes in the udev rules key format.
| A rule to match, can be composed by the attributes of the device
| and the attributes from one single parent device.
| 
| ...
| 
| looking at parent device '/devices/pci0000:00/0000:00:1d.7/usb1/1-3/1-3.4':
|     KERNELS=="1-3.4"
|     SUBSYSTEMS=="usb"
|     DRIVERS=="usb"
|     ATTRS{authorized}=="1"
|     ATTRS{avoid_reset_quirk}=="0"
|     ATTRS{bConfigurationValue}=="1"
|     ATTRS{bDeviceClass}=="00"
|     ATTRS{bDeviceProtocol}=="00"
|     ATTRS{bDeviceSubClass}=="00"
|     ATTRS{bMaxPacketSize0}=="8"
|     ATTRS{bMaxPower}=="90mA"
|     ATTRS{bNumConfigurations}=="1"
|     ATTRS{bNumInterfaces}==" 1"
|     ATTRS{bcdDevice}=="0600"
|     ATTRS{bmAttributes}=="a0"
|     ATTRS{busnum}=="1"
|     ATTRS{configuration}==""
|     ATTRS{devnum}=="10"
|     ATTRS{devpath}=="3.4"
|     ATTRS{idProduct}=="6001"
|     ATTRS{idVendor}=="0403"
|     ATTRS{ltm_capable}=="no"
|     ATTRS{manufacturer}=="FTDI"
|     ATTRS{maxchild}=="0"
|     ATTRS{product}=="FT232R USB UART"
|     ATTRS{quirks}=="0x0"
|     ATTRS{removable}=="unknown"
|     ATTRS{serial}=="A7039N11"
|     ATTRS{speed}=="12"
|     ATTRS{urbnum}=="27"
|     ATTRS{version}==" 2.00"
| 
| ...

| $ udevadm info -a -p $(udevadm info -q path -n /dev/ttyUSB1)
| 
| Udevadm info starts with the device specified by the devpath and then
| walks up the chain of parent devices. It prints for every device
| found, all possible attributes in the udev rules key format.
| A rule to match, can be composed by the attributes of the device
| and the attributes from one single parent device.
| 
| ...
| 
| looking at parent device '/devices/pci0000:00/0000:00:1d.7/usb1/1-3/1-3.2':
|     KERNELS=="1-3.2"
|     SUBSYSTEMS=="usb"
|     DRIVERS=="usb"
|     ATTRS{authorized}=="1"
|     ATTRS{avoid_reset_quirk}=="0"
|     ATTRS{bConfigurationValue}=="1"
|     ATTRS{bDeviceClass}=="ff"
|     ATTRS{bDeviceProtocol}=="00"
|     ATTRS{bDeviceSubClass}=="00"
|     ATTRS{bMaxPacketSize0}=="8"
|     ATTRS{bMaxPower}=="96mA"
|     ATTRS{bNumConfigurations}=="1"
|     ATTRS{bNumInterfaces}==" 1"
|     ATTRS{bcdDevice}=="0254"
|     ATTRS{bmAttributes}=="80"
|     ATTRS{busnum}=="1"
|     ATTRS{configuration}==""
|     ATTRS{devnum}=="11"
|     ATTRS{devpath}=="3.2"
|     ATTRS{idProduct}=="7523"
|     ATTRS{idVendor}=="1a86"
|     ATTRS{ltm_capable}=="no"
|     ATTRS{maxchild}=="0"
|     ATTRS{product}=="USB2.0-Serial"
|     ATTRS{quirks}=="0x0"
|     ATTRS{removable}=="unknown"
|     ATTRS{speed}=="12"
|     ATTRS{urbnum}=="25"
|     ATTRS{version}==" 1.10"
| 
| ...
   
参考
....

http://qiita.com/caad1229/items/309be550441515e185c0
