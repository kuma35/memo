.. -*- coding: utf-8; mode: rst; -*-

======================================
CAPSキーをCTRLキーにする(Ubuntu 15.10)
======================================


CAPSキーをCTRLキーにする(Ubuntu 15.10)(on X Window)
---------------------------------------------------

キー追加
| dconf reset /org/gnome/settings-daemon/plugins/keyboard/active

Caps Lockキーの設定は/org/gnome/desktop/input-sources/xkb-optionsで設定するのでまず現在の値を見る。

| dconf read /org/gnome/desktop/input-sources/xkb-options

デフォルトは未設定で、この場合だとCaps LockキーはCaps Lockとして、CtrlキーはCtrlとしてそれぞれ動作する。
この値を目的に応じて設定する。

Caps LockキーをCtrlキーに割り当てる(元々のCtrlキーはそのままCtrlとして動作する)

| dconf write /org/gnome/desktop/input-sources/xkb-options "['ctrl:nocaps']"

参考
....

http://l-w-i.net/t/ubuntu/key_002.txt


CAPSキーをCTRLキーにする(Ubuntu 15.10)(console)
-----------------------------------------------

/etc/default/keyboard

| # XKBOPTIONS=""
| XKBOPTIONS="ctrl:nocaps"            # CapsLock --> Ctrl
| # XKBOPTIONS="ctrl:swapcaps"        # CapsLock <-> Ctrl

変更を適用

% sudo dpkg-reconfigure -phigh console-setup

参考
....

http://lambdalisue.hatenablog.com/entry/2013/09/27/212118
