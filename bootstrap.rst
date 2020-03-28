.. -*- coding: utf-8; mode: rst; -*-

bootstrap tips
==============

2016年07月19日

モーダルダイアログへのデータ渡し
--------------------------------

modalでdata-\*を渡すときはon('click')...$(#modal).modal()では渡らなかった。
こちらでイベント定義せずにdata-toggle,data-targetを定義しbootstrap側で予め定義されたイベントに任せると上手く渡った。

