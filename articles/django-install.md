---
title: "【Django】開発環境構築" # 記事のタイトル
emoji: "🚀" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア記事
topics: ["python", "django"] # タグ。["markdown", "rust", "aws"]のように指定する
published: false # 公開設定（falseにすると下書き）
---
## はじめに
以前、Djangoの勉強をした記録を公開したいと思います。
下記の画面が表示されるまでを解説したいと思います。
![Djangoインストール](/images/django-install.png)

### 対象読者
- APIで郵便番号から住所を取得する方法が分からない方

### この記事でわかること
- APIで郵便番号から住所を取得する方法が分かる


### 前提条件
- Python 3.9.10
- django 4.1.4

# 手順解説
## 1. ライブラリーのインストール
### 1. djangoライブラリーをインストールする
下記コマンドでインストールする
```bash
pip install django
```
### 2. djangoライブラリーを確認する
`Version: 4.1.4`がインストールされている事が確認できます。
```bash
$ pip show django
Name: Django
Version: 4.1.4
Summary: A high-level Python web framework that encourages rapid development and clean, pragmatic design.
Home-page: https://www.djangoproject.com/
Author: Django Software Foundation
Author-email: foundation@djangoproject.com
License: BSD-3-Clause
Location: /home/furuta/.local/lib/python3.9/site-packages
Requires: asgiref, sqlparse
Required-by: 
```

## 2. 設定ファイルの作成
Djangoプロジェクトの設定を管理するためのconfigを作成します。
下記コマンドでカレントディレクトリ内にconfigファイルを作成します。
```bash
django-admin startproject config .
```
:::message
設定ファイルの作成コマンドのテンプレートは、下記の通りです。
```bash
django-admin startproject ファイル名
```
:::


## configファイルの作成
```bash
src
└─ Django
     ├─ config
     │   ├─ __init__.py
     │   ├─ asgi.py
     │   ├─ settings.py
     │   ├─ urls.py
     │   └─ wsgi.py
     ├─ db.sqlite3
     └─ manage.py
```

## アプリケーションの作成
```bash
python manage.py startproject Study
```
### 1. djangoライブラリーをインストールする
下記コマンドでインストールする
```bash
src
└─ Django
	├── Study
	│   ├── __init__.py
	│   ├── admin.py
	│   ├── apps.py
	│   ├── migrations
	│   │   └── __init__.py
	│   ├── models.py
	│   ├── tests.py
	│   └── views.py
	├── config
	│   ├── __init__.py
	│   ├── asgi.py
	│   ├── settings.py
	│   ├── urls.py
	│   └── wsgi.py
	├── db.sqlite3
	└── manage.py
```

:::message
設定ファイルの作成コマンドは、下記の通りです。
```bash
django-admin startproject ファイル名
```
:::

## ローカル環境で起動する
### 1. 起動コマンドの実行
`python manage.py runserver`でdjangoを起動します
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

### 2. ブラウザ上でアプリケーションを確認する
`http://127.0.0.1:8000/`をブラウザのURL欄に入力しアプリケーションを表示させます。
![Djangoインストール](/images/django-install.png)

## おわりに
requestsライブラリーを初めて触りましたが
今回の学習でrequestsライブラリーの使い方を理解する事が出来ました。

他にもAPIもあるので、色々触って遊びたいと思います。

