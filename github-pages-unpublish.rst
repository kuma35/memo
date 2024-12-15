.. -*- coding: utf-8; mode: rst; -*-

.. index:: github; github pages; github actions;

古い github page を unpublish したら復活できなくなった
======================================================

2024年12月15日

経緯
----

github の 当該リポジトリの Settings の Pages (Github Pages)で
うっかり「Additional site options」→「 Unpublish site」したら、金輪際復活できなくなった。

当該リポジトリのトップページの About の設定(歯車マーク)から
「Use your GitHub Pages website」オプションをOFF/ONしても復活しない。

結論
----

「Github Actions」 で明示的に Static_HTML を追加する。「Actions」→「New workflow」 Pages カテゴリの
Static HTML の「Configure」ボタン押下。
static.yml は一切いじらなくても良かったです。

理由？
------

Github Actions が無い時代に設定した Github Pages はユーザーから見えないところでStatic_HTML アクションを動かしていて、
Unpulish すると裏でこっそり設定してる Static_HTML アクションを削除してしまうっぽい。
その裏バージョンを再度設定する手段は無いっぽいので、 Github Action で Static_HTML アクションを明示的に設定する必要がある。

…のだと思うたぶん。

