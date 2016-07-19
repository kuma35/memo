.. -*- coding: utf-8; mode: rst; -*-

beaker tips
===========

セッションID
------------

.. code-block:: python
   
   session = request.environ.get('beaker.session')
   logger.debug(request.get_cookie(session_opts['session.key'])))
   logger.debug(pformat(session.id))

session.id でクッキーに設定する予定のと同じid文字列が得られる。
get_cookieは初回、まだクッキーを設定していない時は'None'を返す。

