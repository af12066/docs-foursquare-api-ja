.. -*- coding: utf-8 -*-

.. _versioning-internationalization:

バージョンと国際化
==================

.. _versionig:

バージョン
----------

.. All API requests require two special parameters that give developers control over the kind of response that Foursquare returns. These required parameters are:

すべての API リクエストには Foursquare が返すレスポンスの種類を操作するための 2 つの特別なパラメータが必要です。
必要な形式は次のとおりです:

.. A v parameter, which is a date that essentially represents the "version" of the API you expect from Foursquare
.. An m parameter, which specifies whether you want Swarm- or Foursquare-style responses.

1. ``v`` パラメータ: Foursquare から要求する API のバージョンを表す日付です。
2. ``m`` パラメータ: Swarm または Foursquare ライクのレスポンスのどちらを使用するかを指定します。

.. You should send these parameters with every API request, including POST requests. Thus, a valid, fully-formed API request that includes these params looks like:

POST リクエストを含むすべての API リクエストにこれらのパラメータを含める必要があります。
したがって、これらのパラメータを含めた完全な API リクエストは次のようになります::

    https://api.foursquare.com/v2/venues/search
        ?client_id=CLIENT_ID
        &client_secret=CLIENT_SECRET
        &ll=40.7,-74
        &query=sushi
        &v=YYYYMMDD
        &m=foursquare

.. the-v-arameter:

``v`` パラメータ
----------------

.. The v param is designed to give developers the freedom to adapt to Foursquare API changes on their own schedule. The value of the v param is essentially a date in YYYYMMDD format that lets you tell us "I'm prepared for API changes up to this date." As a concrete example, suppose we introduce a change in the future, that renames all our id fields to foo. If your API requests are passing in v=YYYYMMDD or smaller values, these fields will still be called id. However, once you change your parameters to a new date or greater, you will see foo in our responses instead.

``v`` パラメータは開発者が Foursquare API の変更を各自の計画で自由に適用できるように設計されています。
``v`` パラメータの値は ``YYYYMMDD`` の日付フォーマットとなっており、「この日付までの API の変更に対応しています」ということを意味しています。
具体的な例として、将来の変更を導入してすべての `id` フィールドが `foo` となった場合を考えてみます。
API リクエストが ``v=YYYYMMDD`` またはそれより前の値を渡している場合、 ``id`` フィールドが呼ばれたままになります。
しかし、パラメータの値をより新しい日付に更新した場合は ``foo`` フィールドが採用されます。

.. We recommend setting a single date across all your API calls and launching your app with API calls using this date. Our suggestion is to 1) increase this date once every few months, 2) check if Foursquare has made any changes since your old version, and 3) modify your implementation to accommodate for these changes.

すべての API コールで 1 つの日付を設定し、この日付で API 呼び出しによるアプリケーションの起動をお勧めします。
1) 数ヶ月に一度日付を更新する 2) Foursquare が古いバージョンから変更を加えたかチェックする 3) これらの変更に対応するように実装を変更する ことを提案します。

.. Under no circumstances should you always pass in the current date. Please choose a single date and don't increase it until you are ready to accept any possible changes we've made. Note also that the version parameter passed in with each request has nothing to do with the v2 as part of our API's URL path—that should always be v2.

**どのような場合でもつねに現在の日付を渡すべきではありません。**
一つの日付を選択し、起こりうる変更に対応する準備が整うまで値を更新しないでください。
また、リクエストごとに渡されるバージョンパラメータは API の URL パスの一部としての ``v2`` とは無関係に ``v2`` であるべきです。

.. _deprecation:

非推奨
^^^^^^

.. We will occasionally need to deprecate certain parts of the API and API versions as Foursquare continues to grow and evolve. Responses to requests using deprecated versions and endpoints will contain useful information in the meta section of the response—look out for an errorType of deprecated. Over time, we will phase out support for legacy behavior and versions of the API, but we expect to allow at least several months for any transitions.

Foursquare の成長と発展にともなって、API やそのバージョンの特定の部分を非推奨とする場合があります。
非推奨のバージョンやエンドポイントを使用した際のリクエストに対するレスポンスは、 ``meta`` セクションにそれらに関する情報が含まれます -- ``errorType`` の ``deprecated`` を確認してください。
時間とともに従来の動作や API のバージョンのサポートを段階的に廃止しますが、移行には少なくとも数ヶ月はかかる見込みです。

.. To warn developers about impending changes, we will often email developers who will be affected by the changes as well as conduct "API brownouts" before the changes go live to simulate the effects of the changes. For this reason, please actively monitor the email address associated with your Foursquare developer account.

開発者に対して差し迫る変更について警告するため、変更の影響を受ける開発者に E メールを送信し、変更が実際に反映される前に "API brownouts" を行い変更の影響をシミュレートします。
そのため、開発者アカウントに紐づけられた E メールアドレスを頻繁にチェックしてください。

.. _the-m-parameter:

``m`` パラメータ
----------------

.. Since there is only a single API that powers both Swarm and Foursquare, sometimes it makes sense for the same endpoint to return different information in its response, depending on context. The m (for "mode") param gives developers control over whether they want Swarm- or Foursquare-style API responses—for example, the Users Detail endpoint might return information check-ins with m=swarm but information about a user's tips with m=foursquare.

.. Unless your application evolves significantly, it seems unlikely that you will ever have to change the m param values you pass in.

.. _internationalization:

国際化
------

.. You can specify the locale by setting the Accept-Language HTTP header in your request. Alternatively, you can add a locale=XXX parameter to your request but HTTP header specification is preferred. We currently support en (default), es, fr, de, it, ja, th, tr, ko, ru, pt, and id.

.. If nothing is specified, for geographical entities (e.g., city names), we'll fall back to using the language that's most popular in the country for that venue.

.. Foursquare also supports many country-specific subcategories in our venue categories. "Suggested Countries" are listed in our category tree for categories that we think will only apply in certain countries.

.. seealso::

   Versioning & Internationalization (https://developer.foursquare.com/overview/versioning)
