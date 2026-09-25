---
title: "[Django] 環境構築/インストール" # 記事のタイトル
emoji: "🚀" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア記事
topics: ["python", "django", "初心者向け"] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開:true / 非公開:false
---

## 🌱 はじめに

Django は、Python で Web アプリケーションを作るためのフレームワークです。
この記事では、Django をインストールし、下記の画面を表示するまでの手順を解説します。

![Djangoインストール](/images/articles/django-install/django-install.png)

### 動作確認環境

|項目|バージョン|
|---|---|
|OS|Linux|
|Python|3.9|
|Django|4.1.4|

:::message alert
Django 4.1.4 は記事執筆時点（2022年12月）のバージョンで、Django 4.1 系はすでにサポートが終了しています。これから使う場合は、公式サイトでサポート中のバージョンと、対応する Python のバージョンを確認してください。
:::

@[card](https://www.djangoproject.com/download/)

@[card](https://docs.djangoproject.com/ja/stable/faq/install/)

### プロジェクトとアプリケーション

Django では、次の 2 つを作成します。

|名前|役割|本記事での名前|
|---|---|---|
|**プロジェクト**|サイト全体の設定をまとめる入れ物|`config`|
|**アプリケーション**|機能ごとのまとまり。1 つのプロジェクトに複数追加できる|`Study`|

## 🌱 0. 作業ディレクトリの作成

下記のコマンドで作業ディレクトリを作成し、移動します。ディレクトリ名は任意です。以降のコマンドは、すべてこのディレクトリで実行します。

```bash
mkdir -p src/Django
cd src/Django
```

```text
src
└─ Django
```

## 🌱 1. ライブラリのインストール

:::message
Python のパッケージは、プロジェクトごとに仮想環境（venv）を作ってインストールすると、ほかのプロジェクトとのバージョンの衝突を防げます。

```bash
python -m venv venv
source venv/bin/activate  # Windows の場合は venv\Scripts\activate
```

:::

1. `django`ライブラリを下記コマンドでインストールする
   ※記事と同じバージョンを使うため、バージョンを指定しています。

```bash
pip install django==4.1.4
```

2. `Version: 4.1.4`がインストールされていることを確認する
   ※行頭の`$`はプロンプトを表すため、入力は不要です。

```bash
$ pip show django
Name: Django
Version: 4.1.4
Summary: A high-level Python web framework that encourages rapid development and clean, pragmatic design.
Home-page: https://www.djangoproject.com/
Author: Django Software Foundation
Author-email: foundation@djangoproject.com
License: BSD-3-Clause
Location: /home/user/.local/lib/python3.9/site-packages
Requires: asgiref, sqlparse
Required-by:
```

## 🌱 2. プロジェクトの作成

Django プロジェクトの設定を管理する`config`ディレクトリを作成します。

1. 下記コマンドで、`Django`ディレクトリ内に`config`ディレクトリを作成する
   ※末尾の`.`は、カレントディレクトリにプロジェクトを作成する指定です。`.`を付けないと、ディレクトリが 1 段深くなります。

```bash
django-admin startproject config .
```

2. `config`ディレクトリの作成後のファイル構成を確認する

```text
src
└─ Django
    ├─ config
    │   ├─ __init__.py
    │   ├─ asgi.py
    │   ├─ settings.py
    │   ├─ urls.py
    │   └─ wsgi.py
    └─ manage.py
```

## 🌱 3. アプリケーションの作成

プロジェクト（`config`）の中に、アプリケーション`Study`を作成します。

1. `Study`というアプリケーションを作成するために、下記コマンドを実行する

```bash
python manage.py startapp Study
```

2. アプリケーションの作成後のファイル構成を確認する

```text
src
└─ Django
    ├─ Study
    │   ├─ __init__.py
    │   ├─ admin.py
    │   ├─ apps.py
    │   ├─ migrations
    │   │   └─ __init__.py
    │   ├─ models.py
    │   ├─ tests.py
    │   └─ views.py
    ├─ config
    │   ├─ __init__.py
    │   ├─ asgi.py
    │   ├─ settings.py
    │   ├─ urls.py
    │   └─ wsgi.py
    └─ manage.py
```

## 🌱 4. settings.py の編集

作成した`Study`アプリケーションを Django に認識させるため、`config/settings.py`の`INSTALLED_APPS`に、`Study/apps.py`の`StudyConfig`クラスを登録します。

1. `INSTALLED_APPS`のリストに`'Study.apps.StudyConfig',`を追加する

```diff python:config/settings.py
INSTALLED_APPS = [
+    'Study.apps.StudyConfig',
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
]
```

2. 表示言語を日本語、タイムゾーンを日本時間に変更する

```diff python:config/settings.py
# 日本語
- LANGUAGE_CODE = 'en-us'
+ LANGUAGE_CODE = 'ja'

# 日本時間（Asia/Tokyo）
- TIME_ZONE = 'UTC'
+ TIME_ZONE = 'Asia/Tokyo'
```

@[card](https://docs.djangoproject.com/ja/stable/ref/settings/)

## 🌱 5. ローカル環境で起動する

1. `python manage.py migrate`で、Django が標準で使うデータベースのテーブルを作成する
   ※このとき`db.sqlite3`が作成されます。実行しないまま起動すると、`You have 18 unapplied migration(s).`という警告が表示されます。

```bash
python manage.py migrate
```

2. `python manage.py runserver`で Django の開発用サーバーを起動する

```bash
$ python manage.py runserver
Watching for file changes with StatReloader
Performing system checks...

System check identified no issues (0 silenced).

December 30, 2022 - 01:44:09
Django version 4.1.4, using settings 'config.settings'
Starting development server at http://127.0.0.1:8000/
Quit the server with CONTROL-C.
```

3. サーバーを起動したまま、ブラウザの URL 欄に`http://127.0.0.1:8000/`（自分の PC の 8000 番ポート）を入力し、アプリケーションを表示する

:::message
冒頭と同じロケットの画面が表示されれば、起動は成功です。
この画面は、`DEBUG=True`で URL が何も設定されていないときに Django が表示する初期画面です。`Study`アプリケーションの画面ではありません。
:::

![Djangoインストール](/images/articles/django-install/django-install.png)

4. 確認できたら、ターミナルで`Ctrl+C`を押してサーバーを停止する

@[card](https://docs.djangoproject.com/ja/stable/intro/tutorial01/)

## 🌱 まとめ

本記事では、次の手順で Django の初期画面を表示しました。

- `pip install`で Django をインストールする
- `django-admin startproject`でプロジェクトを、`python manage.py startapp`でアプリケーションを作成する
- `settings.py`でアプリケーションを登録し、言語とタイムゾーンを設定する
- `python manage.py migrate`と`python manage.py runserver`で起動する
