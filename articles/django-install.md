---
title: "【Django】環境構築/インストール" # 記事のタイトル
emoji: "🚀" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア記事
topics: ["python", "django", "初心者向け"] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開設定（falseにすると下書き）
---
## はじめに
以前、Djangoの勉強をした記録を公開したいと思います。
下記の画面が表示されるまでを解説したいと思います。
![Djangoインストール](/images/django-install.png)


|  項目  | 内容  |
| ---- | ---- |
|  **対象者**  |  ・Django初学者<br>・Python微経験者  |
|  **伝えたい内容**  |  ・Djangoをインストール方法  |
|  **前提条件**  |  ・Python 3.9.10<br>・django 4.1.4 |

# 手順解説

## 0. ファイル構成の確認
下記のようなファイル構成で始めます
```bash
src
└─ Django
```

## 1. ライブラリーのインストール
djangoライブラリーをインストールします。
1. `djangoライブラリー`を下記コマンドでインストールする
```bash
pip install django
```
2. `Version: 4.1.4`がインストールされている事を確認する
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

1. カレントディレクトリを`Django`に移動する
```bash
cd src/Django
```
2. 下記コマンドで内に`config`ファイルを作成します。
```bash
django-admin startproject config .
```
3. configファイルの作成後のファイル構成を確認する
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

## 3. アプリケーションの作成
アプリケーションのベースとなるファイルを作成します。
1. `Study`というアプリケーションの作成するために、下記コマンドを実行します。
```bash
python manage.py startapp Study
```
2. アプリケーションの作成後のファイル構成を確認する
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

## 4. settings.pyの編集
**アプリケーションの起動**が出来るようアプリケーション側のapps.pyのクラスを追加します。
1. `INSTALLED_APPSの配列`に`'Study.apps.StudyConfig',`を追加します。
```diff python: settings.py
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
2. **言語**と**タイムゾーン**をローカル環境にする
```diff python: settings.py
# 日本時間
- LANGUAGE_CODE = 'en-us'
+ LANGUAGE_CODE = 'ja'

# 東京ゾーン
- TIME_ZONE = 'UTC'
+ TIME_ZONE = 'Asia/Tokyo'
```
## 5. ローカル環境で起動する
アプリケーションを実行します。
1. `python manage.py runserver`でdjangoを起動します
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
2. ブラウザ上でアプリケーションを確認する
`http://127.0.0.1:8000/`をブラウザのURL欄に入力しアプリケーションを表示させます。
:::message
チュートリアルのロケットを飛ばすことが出来ました！
:::
![Djangoインストール](/images/django-install.png)

## おわりに
昔は、チュートリアルのロケットを見るのに1時間以上かかりましたが、
今ではすぐに表示できるようになりました。
次は、Dockerfileで他人でも簡単に共有できるようになりたいです。

