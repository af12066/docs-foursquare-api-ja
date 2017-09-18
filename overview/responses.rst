.. -*- coding: utf-8 -*-

.. _responses-errors:

レスポンス & エラー
===================

.. _responses:

レスポンス
----------

.. All responses will look roughly like

すべてのレスポンスは概ね次のようになります::

    {
      "meta": {
        "code": 200,
         ...errorType and errorDetail...
      },
      "notifications": {
         ...notifications...
      },
      "response": {
         ...results...
      }
    }

.. The errorType is detailed information intended for the application developer. We ask applications to show an appropriate error to their users by handling the error codes listed below. We will add additional types as needed. The deprecated type may be returned even if a request is successful.

``errorType`` はアプリケーション開発者向けの詳細な情報です。以下に示すエラーコードを処理することで、適切なエラーメッセージをユーザに表示左折ことができます。
必要に応じて追加の type を追加する場合があります。
リクエストが成功した場合でも、非推奨な type が返ってくる場合があります。

.. The meta section may also contain a message which provides additional information intended to help developers debug problems.

``meta`` セクションには、開発者がデバッグする際に役立つ追加情報を提供するメッセージも含まれます。

.. Certain requests may also contain notifications as a top-level item based on a just-submitted checkin or other user actions.

特定のリクエストには、チェックインやほかのユーザアクションに基づくトップレベルの項目としての通知も含まれます。

.. To use JSONP, add a callback=XXX parameter to your request and we will respond with XXX(response). In the case of JSONP, we always return 200 (except in the case of 500’s) so the brower will allow the response to be handled by your code, but the true HTTP response code can be obtained from the code in the meta response.

JSONP を用いて、リクエストに ``callback=XXX`` を追記すると XXX（レスポンス）というレスポンスが返ってきます。
JSONP の場合、つねにステータスコード 200 を返すため（例外的に 500 番台の場合もあります）、ブラウザはコードでレスポンスの処理を行えますが、実際の HTTP ステータスコードは ``meta`` レスポンスの ``code`` から取得できます。

.. The content of responses may differ based on the m parameter you pass in with your API request. For more information, see our docs on versioning.

応答の内容は、API リクエストで渡す ``m`` パラメータによって異なる場合があります。
詳細な情報は、 :ref:`versioning-internationalization` のドキュメントを参照ください。

.. _errors:

エラー
------

.. As much as possible, Foursquare attempts to use appropriate HTTP status codes to indicate the general class of problem, and this status code is repeated in the code section of the meta response.

Foursquare は可能な限り問題の一般的なクラスを示すのに適切な HTTP ステータスコードを使用します。
このステータスコードは ``meta`` レスポンスの ``code`` セクションで繰り返されます。

.. 400 (Bad Request)	Any case where a parameter is invalid, or a required parameter is missing. This includes the case where no OAuth token is provided and the case where a resource ID is specified incorrectly in a path.
.. 401 (Unauthorized)	The OAuth token was provided but was invalid.
.. 403 (Forbidden)	The requested information cannot be viewed by the acting user, for example, because they are not friends with the user whose data they are trying to read.
.. 404 (Not Found)	Endpoint does not exist.
.. 405 (Method Not Allowed)	Attempting to use POST with a GET-only endpoint, or vice-versa.
.. 409 (Conflict)	The request could not be completed as it is. Use the information included in the response to modify the request and retry.
.. 500 (Internal Server Error)	Foursquare’s servers are unhappy. The request is probably valid but needs to be retried later.

+-----------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------+
| 400 (Bad Request)           | パラメータが無効か、必須パラメータが欠けています。OAuth トークンが指定されていない場合やリソース ID が正しく指定されていない場合も該当します。|
+-----------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------+
| 401 (Unauthorized)          | OAuth トークンが無効です。                                                                                                                    |
+-----------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------+
| 403 (Forbidden)             | リクエストしようとした情報を参照できません。例えば、友達でないユーザに関するすべての情報を参照しようとした場合に発生します。                  |
+-----------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------+
| 404 (Not Found)             | エンドポイントが存在しません。                                                                                                                |
+-----------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------+
| 405 (Method Not Allowed)    | GET のみのエンドポイントで POST しているか、またはその逆の操作を行なっています。                                                              |
+-----------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------+
| 409 (Conflict)              | リクエストを完了できませんでした。レスポンスに含まれる情報を使用してリクエストを変更し、再びリクエストを行います。                            |
+-----------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------+
| 500 (Internal Server Error) | Foursquare のサーバが不調です。おそらくリクエストは有効ですが、あとで再試行する必要があります。                                               |
+-----------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------+
