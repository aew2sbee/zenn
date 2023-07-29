---
title: "【Django】django rest framework環境構築/インストール" # 記事のタイトル
emoji: "🚁" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア記事
topics: ["python", "django"] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開設定（falseにすると下書き）
---
## はじめに
社内の有志メンバーの活動でDRFを使用したので、導入方法を記録として残しておきます。

`http://127.0.0.1:8000/books/list/`にアクセスして
下記の画面が表示されるまでを解説します
![drf-api-step01.png](/images/drf-api-step01.png)

## 0. ファイル構成の確認
下記のようなファイル構成で始めます
````bash
src
└─ backend
````

## 1. ライブラリーのインストール
1. `django`と`django rest framework`を下記コマンドでインストールする
````bash
pip install django
pip install djangorestframework
````

2. djangoのVersionを確認する
````bash
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
````

3. django rest frameworkのVersionを確認する
````bash
$ pip show djangorestframework
Name: djangorestframework
Version: 3.14.0
Summary: Web APIs for Django, made easy.
Home-page: https://www.django-rest-framework.org/
Author: Tom Christie
Author-email: tom@tomchristie.com
License: BSD
Location: c:\users\furutan\appdata\local\programs\python\python310\lib\site-packages
Requires: django, pytz
Required-by:
````

## 2. 設定ファイルの作成
Djangoプロジェクトの設定を管理するための`config`を作成します。

1. カレントディレクトリを`backend`に移動する
````bash
cd src/backend
````
2. 下記コマンドで内に`config`ファイルを作成します。
````bash
django-admin startproject config .
````
3. configファイルの作成後のファイル構成を確認する
````bash
src
└─ backend
     ├─ config
     │   ├─ __init__.py
     │   ├─ asgi.py
     │   ├─ settings.py
     │   ├─ urls.py
     │   └─ wsgi.py
     ├─ db.sqlite3
     └─ manage.py
````

## 3. アプリケーションの作成
アプリケーションのベースとなるファイルを作成します。
1. 書籍の情報を管理する`books`というアプリケーションの作成するために、下記コマンドを実行します。
````bash
python manage.py startapp books
````
2. アプリケーションの作成後のファイル構成を確認する
````bash
src
└─ backend
	├── books
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
````

## 4. settings.pyの編集
**アプリケーションの起動**が出来るようアプリケーション側のapps.pyのクラスを追加します。
1. `INSTALLED_APPSの配列`に`'books.apps.BooksConfig',`と`'rest_framework',`を追加します。
````diff python: settings.py
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
+    'books.apps.BooksConfig',
+    'rest_framework',
]
````
2. **言語**と**タイムゾーン**をローカル環境にする
````diff python: settings.py
# 日本時間
- LANGUAGE_CODE = 'en-us'
+ LANGUAGE_CODE = 'ja'

# 東京ゾーン
- TIME_ZONE = 'UTC'
+ TIME_ZONE = 'Asia/Tokyo'
````
## 5. ローカル環境で起動する
アプリケーションを実行します。
1. `python manage.py runserver`でdjangoを起動します
````bash
$ python manage.py runserver
Watching for file changes with StatReloader
Performing system checks...

System check identified no issues (0 silenced).

December 30, 2022 - 01:44:09
Django version 4.1.4, using settings 'config.settings'
Starting development server at http://127.0.0.1:8000/
Quit the server with CONTROL-C.
````
2. ブラウザ上でアプリケーションを確認する
`http://127.0.0.1:8000/`をブラウザのURL欄に入力しアプリケーションを表示させます。
無事にDjangoを起動する事が出来ました。
![Djangoインストール](/images/django-install.png)

## 6. models.pyの作成
書籍の情報を管理するテーブル設計を行います
````python: books/models.py
from django.db import models

# Create your models here.
class Books(models.Model):
   # 書籍のタイトル
   title = models.CharField(max_length=100)
   # 登録時の日付/時刻を登録する
   # auto_now_add: 登録時のみ
   created_at = models.DateTimeField(auto_now_add=True)
   # 更新時の日付/時刻を更新する
   updated_at = models.DateTimeField(auto_now=True)
   # 管理者画面で表示するデータをtitleの値を返す
   def __str__(self):
       return self.title
````


## 7. makemigrationsの実行
下記コマンドを実行し、先ほど作成したモデルの`books`を`makemigrations`を行う
> makemigrations を実行することで、Djangoにモデルに変更があったこと(この場合、新しいものを作成しました)を伝え、そして変更を マイグレーション の形で保存することができる。
````bash
python manage.py makemigrations books
````
実行結果は、下記の通りです
````bash
$ python manage.py makemigrations books
Migrations for 'books':
  books\migrations\0001_initial.py
    - Create model Books
````
`books`配下の`migrations`に下記のファイルが作成されます。
- `_init__.py`
- `0001_initial.py`
````diff bash
 .
 └── backend
     ├── books
     │   ├── migrations
+    │   │   ├── __init__.py
+    │   │   └── 0001_initial.py
     │   ├── __init__.py
     │   ├── admin.py
     │   ├── apis.py
     │   ├── apps.py
     │   ├── models.py
     │   ├── serializers.py
     │   ├── tests.py
     │   ├── urls.py
     │   └── views.py
     ├── config
     │   ├── __pycache__
     │   │   ├── __init__.cpython-310.pyc
     │   │   ├── settings.cpython-310.pyc
     │   │   └── urls.cpython-310.pyc
     │   ├── __init__.py
     │   ├── asgi.py
     │   ├── settings.py
     │   ├── urls.py
     │   └── wsgi.py
     ├── db.sqlite3
     └── manage.py
````


## 8. migrateの実行
下記コマンドを実行し、`books`の情報をDBに変更・適用します
> migrateは、自動でデータベーススキーマを管理するためのコマンドです
````bash
python manage.py migrate
````
実行結果は、下記の通りです
````bash
$ python manage.py migrate
Operations to perform:
  Apply all migrations: admin, auth, books, contenttypes, sessions
Running migrations:
  Applying contenttypes.0001_initial... OK
  Applying auth.0001_initial... OK
  Applying admin.0001_initial... OK
  Applying admin.0002_logentry_remove_auto_add... OK
  Applying admin.0003_logentry_add_action_flag_choices... OK
  Applying contenttypes.0002_remove_content_type_name... OK
  Applying auth.0002_alter_permission_name_max_length... OK
  Applying auth.0003_alter_user_email_max_length... OK
  Applying auth.0004_alter_user_username_opts... OK
  Applying auth.0005_alter_user_last_login_null... OK
  Applying auth.0006_require_contenttypes_0002... OK
  Applying auth.0007_alter_validators_add_error_messages... OK
  Applying auth.0008_alter_user_username_max_length... OK
  Applying auth.0009_alter_user_last_name_max_length... OK
  Applying auth.0010_alter_group_name_max_length... OK
  Applying auth.0011_update_proxy_permissions... OK
  Applying auth.0012_alter_user_first_name_max_length... OK
  Applying books.0001_initial... OK
  Applying sessions.0001_initial... OK

````


## 9. serializers.pyの作成
DBへの入出力するデータをバリデーションやJson形式に変換する処理を行う`serializers.py`を作成します。
※新規で`serializers.py`をファイル作成を行う
````python: books/serializers.py
from rest_framework import serializers
from books.models import Books
from datetime import datetime

class BooksSerializer(serializers.ModelSerializer):

    created_at = serializers.DateTimeField(format="%Y-%m-%d %H:%M", read_only=True)
    updated_at = serializers.DateTimeField(format="%Y-%m-%d %H:%M", read_only=True)

    class Meta:
        model = Books
        fields = ['id', 'title', 'created_at', 'updated_at']
````

## 8. viewsの作成
CRUD操作を行うために、`views.py`を作成する
````python: books/views.py
from django.shortcuts import render
from .models import Books
from rest_framework import viewsets
from .serializers import BooksSerializer

# Create your views here.
class BooksViewSet(viewsets.ModelViewSet):
    queryset = Books.objects.all()
    serializer_class = BooksSerializer 
````

## 9. urls.pyの作成
APIのURLの設定を行う為、`urls.py`を作成する
````diff python: config/urls.py
  from django.contrib import admin
  from django.urls import path
+ from django.conf.urls import include

  urlpatterns = [
     path('admin/', admin.site.urls),
+    path('books/', include('books.urls'))
 ]

````
````python: books/urls.py
from django.urls import path
from django.conf.urls import include
from rest_framework import routers
from .views import BooksViewSet

router = routers.DefaultRouter()
router.register('list', BooksViewSet)

urlpatterns = [
    path('', include(router.urls))
]

````

## 動作確認
### 1. 初期状態を確認する
アプリケーションを実行します。
1. 下記コマンドを実行し、djangoを起動します
````bash
python manage.py runserver
````
2. `http://127.0.0.1:8000/books/list/`にアクセスする
3. まだデータを追加していないので、`[]`が確認出来る
![drf-api-step01.png](/images/drf-api-step01.png)

### 2. データを追加する(Create)
1. `Title`欄に書籍のタイトルを入力する
2. `POST`ボタンをクリックする
    ![drf-api-step02.png](/images/drf-api-step02.png)
3. さきほど`[]`だったのが、データが追加されたことが確認出来ます。
![drf-api-step03.png](/images/drf-api-step03.png)

### 3. データを更新する(Update)
1. 追加したデータの`id`をURLに追加し、`http://127.0.0.1:8000/books/list/1/`にアクセスする
2. タイトルを`リーダブルコード`から`新リーダブルコード`に変更する
3. `POST`ボタンをクリックする
![drf-api-step04.png](/images/drf-api-step04.png)
4. `Title`と`updated_at`が更新されていることを確認出来る
※`created_at`は、設定ミスで更新されてしまいました。
![drf-api-step05.png](/images/drf-api-step05.png)

### 2. データを削除する(Delete)
1. `http://127.0.0.1:8000/books/list/1/`にアクセスする
2. `DELETE`ボタンをクリックする
![drf-api-step06.png](/images/drf-api-step06.png)
3. ポップアップの`Delete`に対してもボタンをクリックする
![drf-api-step07.png](/images/drf-api-step07.png)
4. 初期状態と同じデータが`[]`になる
![drf-api-step01.png](/images/drf-api-step01.png)

## おわりに
django rest frameworkの環境構築の記事には、見つからなくて大変でした。
結局、Udemyを購入して学習しました。

