---
title: "[Django] db.sqlite3からpostgresqlに変更する方法" # 記事のタイトル
emoji: "🚀" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア記事
topics: ["python", "django", "初心者向け", "postgresql"] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開:true / 非公開:false
---

## 🌱 はじめに

`Django`のデフォルト DB(`db.sqlite3`)から`postgresql`に変更する方法を学習しましたので、執筆します。

### 前提条件

下記記事の作業が完了している前提です。
@[card](https://zenn.dev/aew2sbee/articles/django-install)

## 🌱 1. psycopg2-binary をインストールする

psycopg2-binary とは？

> Python プログラムと PostgreSQL データベースとの間で通信を行うためのライブラリです。

1. 下記コマンドを実行し、インストールする

```bash
pip install psycopg2-binary
```

2. インストールされているか確認する

```bash
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

1. 下記のように変更する

```diff python:settings.py
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

## 🌱 おわりに

変更結果後、ローカルの Postgres に接続できるか確認してみてください。
