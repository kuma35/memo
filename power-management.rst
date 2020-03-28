.. -*- coding: utf-8; mode: rst; -*-


電源管理
========

2016年03月16日

ノートPCの蓋を閉じてもサスペンドしない
--------------------------------------

/etc/systemd/logind.conf

	HandleLidSwitch=ignore

参考
....

http://qiita.com/tukiyo3/items/9db97f9ffea8a26b364b

サスペンドキーとハイバネーションキーを無効にする
------------------------------------------------

/etc/systemd/logind.conf

HandleSuspendKey
	サスペンドキーが押された時に行う動作を定めます。
	
HandleHibernateKey
	ハイバネートキーが押された時に行う動作を定めます。

サスペンドキーとハイバネーションキーを無効にします。
	
	| HandleSuspendKey=ignore
	| HandleHibernateKey=ignore

参考
....

https://wiki.archlinuxjp.org/index.php/%E9%9B%BB%E6%BA%90%E7%AE%A1%E7%90%86
