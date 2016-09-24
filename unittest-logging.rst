.. -*- coding: utf-8; mode: rst; -*-

Python unittestの中でloggingを使う
==================================

環境
----

Python 3.5.2(Ubuntu 16.04LTS(i386))

in setUp()
----------

setUp()の中では通常通りloggerを開く。streamHandlerだと何も出ないので
FileHandlerなどで他所へ出力してください。この例ではlog_fileで指定した
ファイルへ出力しています。

.. literalinclude:: unittest-logging-test_CmdServo.py
   :pyobject: TestInfo.setUp

tearDown()で扱えるよう、shとloggerをself.sh、self.loggerとします。

in tearDown()
-------------

sh.close()して、loggerからshをremoveHandler()します。

sh.close()しないと2つ目以降のtestを実行する時にResouce warnng 
unclose fileと怒られます(実行はできるが、tail -f で追えない)。

Handlerをremoveしないと、当該suite内でテストを実行するたびに
Handlerが追加されてログ出力がダブります。

.. literalinclude:: unittest-logging-test_CmdServo.py
   :pyobject: TestInfo.tearDown

注意
----
setUp()とtearDown()なので、当然ながら
各テストごとにログのセットとクローズが走ります。
