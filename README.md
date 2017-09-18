# Foursquare API 非公式日本語ドキュメント

[![Documentation Status](https://readthedocs.org/projects/docs-foursquare-api-ja/badge/?version=latest)](http://docs-foursquare-api-ja.readthedocs.io/ja/latest/?badge=latest)

## ローカルでのビルド

以下のコマンドを実行して、Sphinx と依存テーマのインストールを行います:

```bash
$ pip install -r requirements.txt
```

プロジェクトのルートで以下のコマンドを実行することで、ルートの `_build` ディレクトリに HTML が生成されます。

```bash
$ sphinx-build -b html . _build
```

## Read the Docs

ドキュメントは以下の Read the Docs に反映されます:  
http://docs-foursquare-api-ja.readthedocs.io/ja/latest/

master ブランチが更新されると自動でビルドが走ります。
