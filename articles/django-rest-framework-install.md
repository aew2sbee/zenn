---
title: "[Django] Django REST framework環境構築/インストール" # 記事のタイトル
emoji: "🚀" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア記事
topics: ["python", "django", "初心者向け"] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開:true / 非公開:false
---

## 🌱 はじめに

社内の有志メンバーの活動で Django REST framework（以下 DRF）を使用したので、導入方法を記録として残しておきます。

DRF は、Django で Web API（データを JSON 形式でやり取りする仕組み）を作りやすくするライブラリです。
本記事では、書籍データの作成・取得・更新・削除（CRUD）ができる API を作り、`http://127.0.0.1:8000/books/list/`にアクセスして下記の画面が表示されるまでを解説します。

![書籍一覧APIの画面](/images/articles/django-rest-framework-install/drf-api-step01.png)

### 動作確認環境

|項目|バージョン|
|---|---|
|Python|3.9 / 3.10|
|Django|4.1.4|
|DRF|3.14.0|

:::message alert
上記は記事執筆時点（2022年12月）のバージョンで、Django 4.1 系はすでにサポートが終了しています。これから使う場合は、公式サイトでサポート中のバージョンと、対応する Python のバージョンを確認してください。
:::

@[card](https://www.djangoproject.com/download/)

@[card](https://www.django-rest-framework.org/#installation)

### プロジェクトとアプリケーション

Django では、次の 2 つを作成します。

|名前|役割|本記事での名前|
|---|---|---|
|**プロジェクト**|サイト全体の設定をまとめる入れ物|`config`|
|**アプリケーション**|機能ごとのまとまり。1 つのプロジェクトに複数追加できる|`books`|

## 🌱 0. 作業ディレクトリの作成

下記のコマンドで作業ディレクトリを作成し、移動します。ディレクトリ名は任意です。以降のコマンドは、すべてこのディレクトリで実行します。

```bash
mkdir -p src/backend
cd src/backend
```

```text
src
└─ backend
```

## 🌱 1. ライブラリーのインストール

:::message
Python のパッケージは、プロジェクトごとに仮想環境（venv）を作ってインストールすると、ほかのプロジェクトとのバージョンの衝突を防げます。

```bash
python -m venv venv
source venv/bin/activate  # Windows の場合は venv\Scripts\activate
```

:::

1. `django`と`djangorestframework`を下記コマンドでインストールする
   ※記事と同じバージョンを使うため、バージョンを指定しています。

```bash
pip install django==4.1.4
pip install djangorestframework==3.14.0
```

2. Django のバージョンを確認する
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

3. DRF のバージョンを確認する

```bash
$ pip show djangorestframework
Name: djangorestframework
Version: 3.14.0
Summary: Web APIs for Django, made easy.
Home-page: https://www.django-rest-framework.org/
Author: Tom Christie
Author-email: tom@tomchristie.com
License: BSD
Location: c:\users\user\appdata\local\programs\python\python310\lib\site-packages
Requires: django, pytz
Required-by:
```

## 🌱 2. プロジェクトの作成

Django プロジェクトの設定を管理する`config`ディレクトリを作成します。

1. 下記コマンドで、`backend`ディレクトリ内に`config`ディレクトリを作成する
   ※末尾の`.`は、カレントディレクトリにプロジェクトを作成する指定です。`.`を付けないと、ディレクトリが 1 段深くなります。

```bash
django-admin startproject config .
```

2. `config`ディレクトリの作成後のファイル構成を確認する

```text
src
└─ backend
    ├─ config
    │   ├─ __init__.py
    │   ├─ asgi.py
    │   ├─ settings.py
    │   ├─ urls.py
    │   └─ wsgi.py
    └─ manage.py
```

## 🌱 3. アプリケーションの作成

1. 書籍の情報を管理する`books`というアプリケーションを作成するために、下記コマンドを実行する

```bash
python manage.py startapp books
```

2. アプリケーションの作成後のファイル構成を確認する

```text
src
└─ backend
    ├─ books
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

作成した`books`アプリケーションと DRF を Django に認識させるため、`config/settings.py`の`INSTALLED_APPS`に登録します。あわせて、言語とタイムゾーンを日本向けに設定します。

1. `INSTALLED_APPS`のリストに`'books.apps.BooksConfig',`と`'rest_framework',`を追加する

```diff python:config/settings.py
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

## 🌱 5. ローカル環境で起動する

1. `python manage.py runserver`で Django の開発用サーバーを起動する
   ※この時点では`migrate`（8 章で実行）をしていないため、`You have 18 unapplied migration(s).`という警告が表示されますが、問題ありません。

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

2. サーバーを起動したまま、ブラウザの URL 欄に`http://127.0.0.1:8000/`を入力し、下記のロケットの画面が表示されることを確認する
   ![Djangoの初期画面](/images/articles/django-rest-framework-install/django-install.png)
3. 確認できたら、ターミナルで`Ctrl+C`を押してサーバーを停止する

## 🌱 6. models.py の作成

書籍の情報を管理するテーブルを設計します。
`models.py`に書いたクラス 1 つがデータベースのテーブル 1 つに、各フィールドが列に対応します。
※models の書き方については下記記事を参考にしてください。

@[card](https://zenn.dev/aew2sbee/articles/django-rest-framework-models)

```python:books/models.py
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
```

## 🌱 7. makemigrations の実行

`makemigrations`は、モデルの変更内容をマイグレーションファイル（データベースの変更内容を記録した設計図）として保存するコマンドです。

> makemigrations を実行することで、Django にモデルに変更があったこと(この場合、新しいものを作成しました)を伝え、そして変更を マイグレーション の形で保存することができます。

出典: https://docs.djangoproject.com/ja/4.1/intro/tutorial02/

下記コマンドを実行し、先ほど作成した`books`アプリケーションのモデルに対して`makemigrations`を実行します。

```bash
python manage.py makemigrations books
```

実行結果は、下記のとおりです。

```bash
$ python manage.py makemigrations books
Migrations for 'books':
  books\migrations\0001_initial.py
    - Create model Books
```

`books`配下の`migrations`に、`0001_initial.py`が作成されます。

```diff text
 src
 └─ backend
     ├─ books
     │   ├─ __init__.py
     │   ├─ admin.py
     │   ├─ apps.py
     │   ├─ migrations
+    │   │   ├─ 0001_initial.py
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
     ├─ db.sqlite3
     └─ manage.py
```

## 🌱 8. migrate の実行

`migrate`は、マイグレーションファイルの内容を実際のデータベースに反映するコマンドです。
下記コマンドを実行し、`books`の情報をデータベースに反映します。

```bash
python manage.py migrate
```

実行結果は、下記のとおりです。

```bash
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

```

@[card](https://docs.djangoproject.com/ja/4.1/ref/django-admin/#migrate)

## 🌱 9. serializers.py の作成

シリアライザーは、モデルのデータと JSON などの形式を相互に変換し、入力値のチェック（バリデーション）を行う仕組みです。
`books`配下に`serializers.py`を新規作成します。

```python:books/serializers.py
from rest_framework import serializers
from books.models import Books

class BooksSerializer(serializers.ModelSerializer):

    # 日時の表示形式を指定し、API から書き換えできない（表示のみの）項目にする
    created_at = serializers.DateTimeField(format="%Y-%m-%d %H:%M", read_only=True)
    updated_at = serializers.DateTimeField(format="%Y-%m-%d %H:%M", read_only=True)

    class Meta:
        model = Books
        fields = ['id', 'title', 'created_at', 'updated_at']
```

## 🌱 10. views.py の作成

CRUD（Create / Read / Update / Delete：作成・取得・更新・削除）の操作を行うために、`views.py`を編集します。
`ModelViewSet`を継承すると、`queryset`（対象データ）と`serializer_class`（変換方法）を指定するだけで、CRUD の処理が自動で用意されます。

```python:books/views.py
from .models import Books
from rest_framework import viewsets
from .serializers import BooksSerializer

# Create your views here.
class BooksViewSet(viewsets.ModelViewSet):
    queryset = Books.objects.all()
    serializer_class = BooksSerializer
```

@[card](https://www.django-rest-framework.org/api-guide/viewsets/)

## 🌱 11. urls.py の作成

API の URL を設定するため、既存の`config/urls.py`を編集し、`books/urls.py`を新規作成します。

```diff python:config/urls.py
 from django.contrib import admin
- from django.urls import path
+ from django.urls import include, path

 urlpatterns = [
     path('admin/', admin.site.urls),
+    path('books/', include('books.urls'))
 ]
```

ルーター（`DefaultRouter`）は、ViewSet から URL を自動で生成する仕組みです。
`config/urls.py`の`books/`と、`books/urls.py`で登録した`list`を組み合わせて、`/books/list/`という URL になります。

```python:books/urls.py
from django.urls import include, path
from rest_framework import routers
from .views import BooksViewSet

router = routers.DefaultRouter()
router.register('list', BooksViewSet)

urlpatterns = [
    path('', include(router.urls))
]
```

@[card](https://www.django-rest-framework.org/api-guide/routers/)

## 🌱 動作確認

DRF には、ブラウザから API を試せる画面（Browsable API）が標準で付いています。この画面を使って、CRUD の動作を確認します。

:::message alert
本記事の API は認証なしで、誰でもデータを作成・更新・削除できます。ローカルでの開発用の構成なので、公開サーバーで使う場合は`DEBUG=False`にし、`permission_classes`などで認証・認可を設定してください。
:::

### 1. 初期状態を確認する

1. 下記コマンドを実行し、Django を起動する

```bash
python manage.py runserver
```

2. `http://127.0.0.1:8000/books/list/`にアクセスする
3. まだデータを追加していないので、`[]`が表示されることを確認する
   ![書籍一覧APIの初期状態](/images/articles/django-rest-framework-install/drf-api-step01.png)

### 2. データを追加する(Create)

1. `Title`欄に書籍のタイトル（例: `リーダブルコード`）を入力する
2. `POST`ボタンをクリックする
   ![タイトルを入力してPOSTする画面](/images/articles/django-rest-framework-install/drf-api-step02.png)
3. さきほどまで`[]`だった一覧に、データが追加されたことを確認する
   ![データが追加された画面](/images/articles/django-rest-framework-install/drf-api-step03.png)

### 3. データを更新する(Update)

1. 追加したデータの`id`を URL に追加し、`http://127.0.0.1:8000/books/list/1/`にアクセスする
   ※何度か追加した場合は、一覧に表示された`id`を使ってください。
2. タイトルを`リーダブルコード`から`新リーダブルコード`に変更する
3. `PUT`ボタンをクリックする
   ![タイトルを変更してPUTする画面](/images/articles/django-rest-framework-install/drf-api-step04.png)
4. `title`と`updated_at`が更新されていることを確認する
   ※スクリーンショットは執筆時の設定ミスにより`created_at`も更新されています。本記事のコード（`auto_now_add=True`と`read_only=True`）では、`created_at`は更新されません。
   ![データが更新された画面](/images/articles/django-rest-framework-install/drf-api-step05.png)

### 4. データを削除する(Delete)

1. `http://127.0.0.1:8000/books/list/1/`にアクセスする
2. `DELETE`ボタンをクリックする
   ![DELETEボタンの位置](/images/articles/django-rest-framework-install/drf-api-step06.png)
3. ポップアップの`Delete`ボタンもクリックする
   ![削除の確認ポップアップ](/images/articles/django-rest-framework-install/drf-api-step07.png)
4. `http://127.0.0.1:8000/books/list/`にアクセスし、データが初期状態と同じ`[]`に戻ったことを確認する
   ![書籍一覧APIの初期状態](/images/articles/django-rest-framework-install/drf-api-step01.png)

## 🌱 おわりに

本記事では、DRF をインストールし、`ModelViewSet`とルーターを使って書籍データの CRUD API を作成しました。

Django REST framework の環境構築の記事が見つからなくて大変でした。
結局、Udemy を購入して学習しました。
