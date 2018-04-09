.. -*- coding: utf-8; mode: rst; -*-

ROS
===

ROSインストール
---------------

for Ubuntu 16.04 LTS(Xenial) i386

それぞれのバージョンが、ディストリビューションの特定のバージョンでしかインストールできないことに注意。

当該ディストリビューション用ソースから作成する場合も同様に束縛される。

よってディストリビューション、バージョン、アーキテクチャに
適合するROSのバージョンをを選ぶ必要がある。

Ubuntu 16.04 LTS(Xenial) i386 なら、選べるので最新のはKinetic

2018/4/2 時点で shadow-repository 使うと

.. code-block:: bash

   $ sudo apt update
   $ sudo apt list ^ros | less
   $ sudo apt list | grep ^ros | less
   $ sudo apt install ros-kinetic-desktop-full


入門
----

.bashrc
.......

~/.bashrc に追加。

.. code-block:: bash

   #for ROS kinetic
   source ${HOME}/catkin_ws/devel/setup.bash

rosdep
......

.. code-block:: bash

   $ sudo rosdep init

initは/etc/apt/apt.source.d/へaptライン追加なのでsudoで実行。初回のみ。

.. code-block:: bash

   $ rosdep update

updateは${HOME}/.ros/内のリストとキャッシュを更新する作業なので
当該userで実行する。

うっかりsudoすると以後updateするときやリストを利用する時に権限無くてエラーになる。その際は sudo chown -R .ros hoge:hoge で権限を修正する。
