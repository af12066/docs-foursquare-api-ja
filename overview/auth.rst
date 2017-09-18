.. -*- coding: utf-8 -*-

.. _connecting:

接続
====

.. _registration:

登録
----

.. Start by registering your app to obtain its Foursquare API credentials.


`アプリケーションを登録 <https://ja.foursquare.com/developers/apps>`_ し、Foursquare API クレデンシャルを取得してください。

.. _access_token:

アクセストークン
----------------

.. Access tokens allow apps to make requests to Foursquare on the behalf of a user. Each access token is unique to the user and consumer key. Access tokens do not expire, but they may be revoked by the user. If your application doesn't require connecting with Foursquare users, you can skip directly to Userless Access.

アクセストークンを使用することで、アプリケーションはユーザの代わりに Foursquare にリクエストを送信できるようになります。
各アクセストークンはユーザと Consumer key で一意の値となっています。
アクセストークンは無期限ですが、ユーザが無効にする場合があります。
アプリケーションが Foursquare ユーザの情報にアクセスする必要がない場合、 :ref:`userless-server-integrations` まで手順をスキップできます。

.. At this time, Foursquare only supports a single authentication mechanism that authorizes an application on behalf of both Foursquare and Swarm. 

**現時点では、Foursquare は Foursquare と Swarm に代わってアプリケーションを認証するための、単一認証機構のみをサポートしています。**

.. _ios-android:

iOS / Android
-------------

.. Native auth is the easiest way for users to connect with Foursquare. Unlike the web-based OAuth flow documented below, our native flow leverages the Foursquare app already installed on your users’ phones, saving users the hassle of re-logging in to Foursquare within your app. Native auth is the only flow that supports users logging in to Foursquare using Facebook.

ネイティブな認証は、ユーザが Foursquare に接続するのにもっとも簡単な方法です。
次に示す Web ベースの OAuth のフローとは異なり、ネイティブなフローはユーザのスマートフォンにすでにインストールされている Foursquare アプリを利用するため、あなたのアプリ内で Foursquare に再ログインする手間を省略できます。
この方法は、唯一 Facebook を経由して Foursquare にログインできるフローです。

.. To use native auth, incorporate our utility classes for iOS or Android into your app. Additional instructions are provided in the repositories' README files.

ネイティブな認証を使用するには、あなたの `iOS <https://github.com/foursquare/foursquare-ios-oauth/>`_ または `Android <https://github.com/foursquare/foursquare-android-oauth>`_ アプリにユーティリティクラスを組み込みます。詳細な使用方法については各リポジトリの README を参照ください。

.. _web-applications-code-flow:

Web アプリケーションのコードフロー
----------------------------------

.. Here is the recommended OAuth 2.0 flow:

推奨される OAuth 2.0 のフローは次のとおりです:

.. Direct users to

1. ユーザに次の URL を与えます::

       https://foursquare.com/oauth2/authenticate
           ?client_id=YOUR_CLIENT_ID
           &response_type=code
           &redirect_uri=YOUR_REGISTERED_REDIRECT_URI

.. (generally done through a link or button)

   （一般的にはリンクやボタンを通して完了します）
   
.. If the user accepts, they will be redirected back to

2. ユーザが承諾したら、以下へリダイレクトします::

       https://YOUR_REGISTERED_REDIRECT_URI/?code=CODE

.. Your server should exchange the code it got in step 2 for an access token. Make a request for

3. アプリケーションサーバにてステップ 2 で取得した CODE をアクセストークンと交換します。**次のリクエストを送信してください**::

       https://foursquare.com/oauth2/access_token
           ?client_id=YOUR_CLIENT_ID
           &client_secret=YOUR_CLIENT_SECRET
           &grant_type=authorization_code
           &redirect_uri=YOUR_REGISTERED_REDIRECT_URI
           &code=CODE

.. The response will be JSON

4. レスポンスは JSON 形式となります::

       { access_token: ACCESS_TOKEN }

.. Save this access token for this user in your database.

5. ユーザのアクセストークンをデータベースに保存してください
 
.. _web-applications-token-flow:

Web アプリケーションのトークンフロー
------------------------------------

.. If you have no substantive server code, you can use the token flow outlined below.

サーバで動作するコードが実質的にない場合、以下に示すトークンフローを使用できます。

.. Redirect users who wish to authenticate to

1. 認証させるユーザを以下へリダイレクトさせます::

    https://foursquare.com/oauth2/authenticate
        ?client_id=CLIENT_ID
        &response_type=token
        &redirect_uri=YOUR_REGISTERED_REDIRECT_URI

.. If a user accepts, they will be redirected back to

2. ユーザが承認したら、以下へリダイレクトします::

    https://YOUR_REGISTERED_REDIRECT_URI/#access_token=ACCESS_TOKEN

.. If register multiple redirect URIs for your app, you can specify which URI to use by changing the value of the redirect_uri parameter. If you enable web connect, your users will be redirected to your first redirect URI.

アプリケーションに複数のリダイレクト URI を登録する場合、``redirect_uri`` パラメータの値を変更することで使用する URI を指定できます。
Web connect を有効にすれば、ユーザは最初のリダイレクト URI へリダイレクトされます。

.. _requests:

リクエスト
----------

.. Once you have an access token, it’s easy to use any of the endpoints, by just adding oauth_token=ACCESS_TOKEN to your GET or POST request. For example, from the command line, you can do

一度アクセストークンを入手すれば、:ref:`api-endpoints` の使用は簡単です。
GET または POST リクエストに ``oauth_token=ACCESS_TOKEN`` を追記してください。
例えば、コマンドラインでは次のようになります::

    curl https://api.foursquare.com/v2/users/self/checkins?oauth_token=ACCESS_TOKEN&v=YYYYMMDD

.. _authentication:

認証
----

.. The examples above use /authenticate instead of /authorize, which what the OAuth protocol specifies.
.. It is worth noting that /authenticate handles both user authentication and app authorization and automatically redirects if a user has already authorized the calling app.
.. Conversely, /authorize will prompt the user to accept the the auth request every time.

上の例では ``/authorize`` の代わりに ``/authenticate`` を使用していますが、これは OAuth プロトコルで指定されているためです。
``/authenticate`` はユーザ認証とアプリケーションの認証の両方を処理し、ユーザがすでにアプリケーションを認証していれば自動的にリダイレクトされることに注意してください。
**一方、** ``/authorize`` **はつねに認証リクエストを承諾するようユーザに指示します。**

.. We encourage web apps to use session cookies to verify a user's identity once the user has been initially authenticated. All embedded webviews inside of Foursquare share the same cookies, so all subsequent interactions can rely on the session cookie to authenticate the user, avoiding server redirects each time the user interacts with the app.

ユーザの初回認証時に Web アプリケーション側でセッション Cookie を使用し、ユーザの身元を確認することをお勧めします。
Foursquare 内部のすべての Webview は同じ Cookie を共有しているため、以後すべての通信はセッション Cookie によってユーザの認証を行い、ユーザがアプリケーションを操作するたびにサーバのリダイレクトを回避できます。


.. _userless-server-integrations:

ユーザレスサーバインテグレーション
----------------------------------

.. Some of our endpoints that don’t pertain to specific user information, such as venues search are enabled for userless access (meaning you don’t need to have a user auth your app for access). To make a userless request, specify your consumer key's Client ID and Secret instead of an auth token in the request URL.

ベニュー検索など特定のユーザの情報に関係しないエンドポイントのいくつかは、ユーザレスアクセスが可能です（ユーザがアクセスのためにアプリケーション内で認証する必要がないということです）。
ユーザレスリクエストを生成するには、リクエスト URL の認証トークンの代わりに consumer key のクライアント ID とシークレットを指定します::

    https://api.foursquare.com/v2/venues/search?ll=40.7,-74&client_id=CLIENT_ID&client_secret=CLIENT_SECRET&v=YYYYMMDD

.. To see what level of permissions each endpoint needs, check out the filters at the top of our endpoints page.

エンドポイントごとにどのレベルの権限が必要か確認するには、 :ref:`api-endpoints` ページの上部のフィルターで確認してください。

.. _display-types:

表示形式
--------

.. By default, Foursquare auto-detects the appropriate type of site to display for the auth dialog. You can force a specific display type by adding display=XXX to your authorize or authenticate URLs. The supported display types are touch, and webpopup. We don't recommend specifying a display type, unless you are using the webpopup display. In webpopup mode, we pop up a new web window for the auth dialog, redirect to the callback in the caller window, and close the popup window after the user authorizes the app.

通常、Foursquare は認証ダイアログに表示する適切なサイトの形式を自動で検出します。
認可（authorize）または認証（authenticate）URL に ``display=XXX`` を追記することで表示形式を指定することも可能です。
サポートされる表示形式は ``touch`` および ``webpopup`` となります。
``webpopup`` ディスプレイを使用する場合を除き、表示形式を指定することをお勧めしません。
``webpopup`` モードでは、ポップアップウィンドウにて認証ダイアログを表示させ、呼び出し元のウィンドウのコールバックにリダイレクトし、アプリケーションの認可後にポップアップウィンドウが閉じられます。

.. _notes-on-oauth:

OAuth に関する注意
------------------

.. Be sure to note that although API requests are against  api.foursquare.com, OAuth token and authorization requests are against  foursquare.com.

API リクエストは ``api.foursquare.com`` に対して行いますが、OAuth トークンと認証リクエストは ``foursquare.com`` に対して行うことに注意してください。

.. One issue you may run into on Android is that Foursquare uses a wildcard SSL cert. For more information, see this Stack Overflow answer.

Android 上で実行するうえでの問題は、Foursquare がワイルドカード SSL 証明書を用いることです。詳細は、`こちらの Stack Overflow の回答 <https://stackoverflow.com/questions/3135679/android-httpclient-hostname-in-certificate-didnt-match-example-com-ex>`_ を参照ください。

.. Although at this time we do not expire OAuth access tokens, you should be prepared for this possibility. Also remember that a user may disconnect via the Foursquare settings page at any time. Using  /authorize will ask the user to re-authenticate their identity and reauthorize your app while giving the user the option to login under a different account.

現時点では OAuth アクセストークンは期限切れではありませんが、期限切れの可能性を考慮してください。
また、ユーザは Foursquare の設定画面からいつでも切断することができます。
``/authorize`` を使用するとユーザを再認証してアプリケーションを再認可する手続きを踏むため、別のアカウントでログインするオプションをユーザに与えることができます。

.. seealso::

   Connecting (https://developer.foursquare.com/overview/auth)
