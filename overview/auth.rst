.. -*- coding: utf-8 -*-

.. Connecting

接続
====

.. Registration

登録
----

.. Start by registering your app to obtain its Foursquare API credentials.


`アプリケーションを登録 <https://ja.foursquare.com/developers/apps>`_ し、Foursquare API クレデンシャルを取得してください。

アクセストークン
----------------

.. Access tokens allow apps to make requests to Foursquare on the behalf of a user. Each access token is unique to the user and consumer key. Access tokens do not expire, but they may be revoked by the user. If your application doesn't require connecting with Foursquare users, you can skip directly to Userless Access.

アクセストークンを使用することで、アプリケーションはユーザの代わりに Foursquare にリクエストを送信できるようになります。
各アクセストークンはユーザと Consumer key で一意の値となっています。
アクセストークンは無期限ですが、ユーザが無効にする場合があります。
アプリケーションが Foursquare ユーザの情報にアクセスする必要がない場合、ユーザレスアクセスまで手順をスキップできます。

.. seealso::

   Connecting (https://developer.foursquare.com/overview/auth)
