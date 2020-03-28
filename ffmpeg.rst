.. -*- coding: utf-8; mode: rst; -*-

ffmpeg
======

2016年03月19日

$ ffmpeg -f video4linux2 -list_formats all -i /dev/video1

ffmpeg version 2.7.6-0ubuntu0.15.10.1 Copyright (c) 2000-2016 the FFmpeg developers

built with gcc 5.2.1 (Ubuntu 5.2.1-22ubuntu2) 20151010

configuration: --prefix=/usr --extra-version=0ubuntu0.15.10.1 --build-suffix=-ffmpeg --toolchain=hardened --libdir=/usr/lib/i386-linux-gnu --incdir=/usr/include/i386-linux-gnu --enable-gpl --enable-shared --disable-stripping --enable-avresample --enable-avisynth --enable-frei0r --enable-gnutls --enable-ladspa --enable-libass --enable-libbluray --enable-libbs2b --enable-libcaca --enable-libcdio --enable-libflite --enable-libfontconfig --enable-libfreetype --enable-libfribidi --enable-libgme --enable-libgsm --enable-libmodplug --enable-libmp3lame --enable-libopenjpeg --enable-openal --enable-libopus --enable-libpulse --enable-librtmp --enable-libschroedinger --enable-libshine --enable-libspeex --enable-libtheora --enable-libtwolame --enable-libvorbis --enable-libvpx --enable-libwavpack --enable-libwebp --enable-libxvid --enable-libzvbi --enable-opengl --enable-x11grab --enable-libdc1394 --enable-libiec61883 --enable-libzmq --enable-libssh --enable-libsoxr --enable-libx264 --enable-libopencv --enable-libx265 --disable-i686

|  libavutil      54. 27.100 / 54. 27.100
|  libavcodec     56. 41.100 / 56. 41.100
|  libavformat    56. 36.100 / 56. 36.100
|  libavdevice    56.  4.100 / 56.  4.100
|  libavfilter     5. 16.101 /  5. 16.101
|  libavresample   2.  1.  0 /  2.  1.  0
|  libswscale      3.  1.101 /  3.  1.101
|  libswresample   1.  2.100 /  1.  2.100
|  libpostproc    53.  3.100 / 53.  3.100

[video4linux2,v4l2 @ 0xa011d00] Raw       :     yuyv422 :           YUYV 4:2:2 : 640x480 1280x720 960x544 800x448 640x360 424x240 352x288 320x240 800x600 176x144 160x120 1920x1080

[video4linux2,v4l2 @ 0xa011d00] Compressed:       mjpeg :          Motion-JPEG : 640x480 1920x1080 1280x720 960x544 800x448 640x360 800x600 432x240 352x288 176x144 320x240 160x120

[video4linux2,v4l2 @ 0xa011d00] Raw       : Unsupported :     YUV 4:2:0 (M420) : 640x480 1280x720 960x544 800x448 640x360 424x240 352x288 320x240 800x600 176x144 160x120 1920x1080


