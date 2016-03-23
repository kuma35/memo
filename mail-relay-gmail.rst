.. -*- coding: utf-8; mode: rst; -*-

mailをgmailにリレーするだけ
===========================

ubuntu 15.10(i386)

メールを全部gmailに放り投げるだけなので、
軽そうなssmtpを使ってみる。

.. code-block:: bash

   $ sudo apt install ssmtp

/etc/ssmtp/ssmtp.confをいじる。

メールアドレス
    hogehoge@gmail.com

パスワード
    fuga1234

とする。

| root=hogehoge@gmail.com
| mailhub=smtp.gmail.com:587
| rewriteDomain=gmail.com
| hostname=gmail.com
| AuthUser=hogehoge@gmail.com
| AuthPass=fuga1234
| AuthMethod=LOGIN
| UseSTARTTLS=YES
| FromLineOverride=YES

.. 平文でパスワード書いちゃうのがちと気持ち悪い。パーミッションは644でいいのかしらん。

再起動等は不要。

テスト
------

test-mail.txt

| To:hogehoge@gmail.com
| From:hogehoge@gmail.com
| Subject:test-mail
| 
| test-mail

.. code-block:: bash

   $ sudo /usr/sbin/sendmail -t < test-mail.txt

メーラーなりWebメールなりでメールが届いていれば成功。

