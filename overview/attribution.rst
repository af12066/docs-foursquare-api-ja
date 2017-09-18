.. -*- coding: utf-8 -*-

.. _attribution-and-linking:

帰属とリンク
============

.. Just as you’d never quote someone without attribution, credit Foursquare when using our data. Under certain circumstances, other terms may apply.

帰属なしに引用できないのと同様に、Foursquare のデータを利用する際にはクレジットが必要です。特定の状況下では、その他の条件が適用される場合があります。

.. _attribution-when-using-foursquare-venues:

Foursquare のベニューを使用する際の帰属
---------------------------------------

.. One of the most popular ways to interact with our API is using our venues database, and certain attribution policies apply when you do. If you aren’t using any venues data (quick check: are you ever hitting a venues/* endpoint?), you can skip straight to Visual Attribution.

API を用いるもっとも一般的な方法はベニューのデータベースを使用することであり、特定の帰属ポリシーが適用されます。
ベニューデータを使用しない場合（クイックチェック: venues/* エンドポイントを使用しますか？）、 :ref:`visual-attribution` までスキップできます。

.. _linking-to-us-basic-data:

リンク: 基本的なデータ
^^^^^^^^^^^^^^^^^^^^^^

.. Give your users the opportunity to learn more about the venues they see in your app. To do this, provide links back to corresponding Foursquare venue pages whenever you display any basic data (name, location, and category) retrieved from our venues database. Common approaches to this include:

.. linking the venue name directly
.. creating your own place detail page and linking that back to us
.. having some other dedicated “More Details” link

.. Use the sample link below as a reference for how to format your URLs:

.. This is a standard web link that degrades nicely to a mobile view, but we also encourage mobile clients to consider using a direct link to the venue detail page in the Foursquare native app. You can find the base URL in the canonicalUrl field in a venue details response, or you could also stitch a URL together with http://foursquare.com/v/ + venueId.

.. Make sure to supply the ref parameter whose value is your app's CLIENT_ID so that we can confirm your app is attributing properly. Don't set rel="nofollow" on your links back to Foursquare.

.. If it absolutely doesn’t make sense to provide in links to Foursquare in your app's UI, you may provide Visual Attribution (see below) in lieu of linking. We expect most apps to be able to support linking.

.. _beyond-basic-data:

基本的なデータの先
^^^^^^^^^^^^^^^^^^

.. _search-result:

検索結果
^^^^^^^^

.. _visual-attribution:

ビジュアル アトリビューション
-----------------------------

.. seealso::

   Attribution & Linking (https://developer.foursquare.com/overview/attribution)
