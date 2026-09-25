---
title: "[Django] db.sqlite3からPostgreSQLに変更する方法" # 記事のタイトル
emoji: "🚀" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア記事
topics: ["python", "django", "初心者向け", "postgresql"] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開:true / 非公開:false
---

## 🌱 はじめに

`Django`のデフォルトの DB（`db.sqlite3`）を`PostgreSQL`に切り替える手順をまとめます。

SQLite はファイル 1 つで動く手軽な DB で、Django のプロジェクトを作成すると最初から使えます。PostgreSQL は別途起動しておくサーバー型の DB で、本番運用や同時アクセスに強いのが特徴です。

:::message
本記事で扱うのは接続先の切り替えまでです。`db.sqlite3`に保存済みのデータは PostgreSQL に移行されません。データを移行する場合は、`python manage.py dumpdata`で書き出し、`python manage.py loaddata`で読み込む方法があります。
:::

### 前提条件

- 下記記事の作業が完了していること

@[card](https://zenn.dev/aew2sbee/articles/django-install)

- PostgreSQL がローカルにインストールされ、`localhost:5432`で起動していること
- PostgreSQL に、本記事の設定で使うユーザー・パスワード・DB（いずれも`postgres`）があること
  ※自分の環境で別の値を設定している場合は、2 章の設定値をその値に置き換えてください。

## 🌱 1. psycopg2-binary をインストールする

psycopg2-binary は、Python のプログラムと PostgreSQL の DB との間で通信を行うためのライブラリです。コンパイル済みのため、pip だけで導入できます。

:::message
psycopg2-binary は開発・テスト向けのパッケージで、本番環境ではソースからビルドする`psycopg2`などの利用が推奨されています。また、新しいバージョンの Django では後継の`psycopg`（バージョン 3）が推奨されています。
:::

@[card](https://www.psycopg.org/docs/install.html)

1. 下記コマンドを実行し、インストールする

```bash
pip install psycopg2-binary
```

2. 下記コマンドを実行し、インストールされているか確認する

```bash
pip show psycopg2-binary
```

下記のように`Name`と`Version`が表示されれば、インストールできています（バージョンは記事執筆時点のものです）。

```text
Name: psycopg2-binary
Version: 2.9.7
Summary: psycopg2 - Python-PostgreSQL Database Adapter
Home-page: https://psycopg.org/
Author: Federico Di Gregorio
Author-email: fog@initd.org
License: LGPL with exceptions
Location: c:\users\user\appdata\local\programs\python\python310\lib\site-packages
Requires:
Required-by:
```

## 🌱 2. `settings.py`の内容を変更する

`config/settings.py`の`DATABASES`を下記のように変更します。

```diff python:config/settings.py
- DATABASES = {
-     'default': {
-         'ENGINE': 'django.db.backends.sqlite3',
-         'NAME': BASE_DIR / 'db.sqlite3',
-     }
- }
+ DATABASES = {
+     'default': {
+         'ENGINE': 'django.db.backends.postgresql',
+         'NAME': 'postgres',
+         'USER': 'postgres',
+         'PASSWORD': 'postgres',
+         'HOST': 'localhost',
+         'PORT': '5432',
+     }
+ }
```

各項目の意味は次のとおりです。

|項目|意味|
|---|---|
|`ENGINE`|使用する DB の種類。PostgreSQL の場合は`django.db.backends.postgresql`|
|`NAME`|接続する DB の名前|
|`USER`|DB に接続するユーザー名|
|`PASSWORD`|DB に接続するユーザーのパスワード|
|`HOST`|DB サーバーの場所。自分の PC の場合は`localhost`|
|`PORT`|DB サーバーのポート番号。PostgreSQL の標準は`5432`|

:::message alert
上記はローカル開発用の設定例です。パスワードを`settings.py`に直接書いたまま公開リポジトリに push しないでください。本番環境では強いパスワードを設定し、環境変数などで管理してください。
:::

@[card](https://docs.djangoproject.com/ja/stable/ref/settings/#databases)

## 🌱 3. マイグレーションを実行して接続を確認する

切り替えた直後の PostgreSQL にはテーブルがないため、下記コマンドでテーブルを作成します。
エラーが出ずに完了すれば、PostgreSQL に接続できています。

```bash
python manage.py migrate
```

## 🌱 おわりに

本記事では、次の手順で Django の DB を PostgreSQL に切り替えました。

- psycopg2-binary をインストールする
- `settings.py`の`DATABASES`を PostgreSQL 用に変更する
- `python manage.py migrate`でテーブルを作成し、接続を確認する
