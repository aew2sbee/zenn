---
title: "【Django】一覧ページをListViewを使おう！" # 記事のタイトル
emoji: "🥟" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア記事
topics: ["python", "django", "ListView"] # タグ。["markdown", "rust", "aws"]のように指定する
published: false # 公開設定（falseにすると下書き）
---
## はじめに
以前、Djangoの勉強をした記録を公開したいと思います。
ListViewで一覧ページを作成する方法までを解説したいと思います。

|  項目  | 内容  |
| ---- | ---- |
|  **対象者**  |  ・ListViewで一覧ページを作成する方法を知りたい方<br>・Python微経験者  |
|  **伝えたい内容**  |  ・ListViewの使い方  |
|  **前提条件**  |  ・Python 3.9.10<br>・django 4.1.4 |

### 結論
|  項目  | 内容  |
| ---- | ---- |
|  **対象者**  |  ・ListViewで一覧ページを作成する方法を知りたい方<br>・Python微経験者  |
|  **伝えたい内容**  |  ・ListViewの使い方  |
|  **前提条件**  |  ・Python 3.9.10<br>・django 4.1.4 |


# 手順解説

## 0. ファイル構成の確認
下記のようなファイル構成で始めます
```python
from django.db import models
# Create your models here.


class Article(models.Model):
    # verbose_name: 管理サイト上で指定の文字列を表示が出来る
    # auto_now_add: 投稿時に現在日時を設定する
    # blank: 投稿時に空白を許容する
    title = models.CharField(max_length=100, verbose_name='タイトル')
    message = models.TextField(verbose_name='本文')
    created_at = models.DateField(auto_now_add=True, blank=True, verbose_name='投稿日')

    def __str__(self):
        # 管理画面の一覧にタイトルを表示できるように設定
        return self.title

    class Meta:
        # 属性と属性値を参照してそのモデルのオブジェクトの扱い方を変更する
        # 管理サイト上のモデルの表記方法を変更する
        verbose_name_plural = "記事"
```

```bash
{% for article in object_list %}
<li>{{ article.title }}</li>
{% endfor %}
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
```python
from django.db import models
# Create your models here.


class Article(models.Model):
    # verbose_name: 管理サイト上で指定の文字列を表示が出来る
    # auto_now_add: 投稿時に現在日時を設定する
    # blank: 投稿時に空白を許容する
    title = models.CharField(max_length=100, verbose_name='タイトル')
    message = models.TextField(verbose_name='本文')
    created_at = models.DateField(auto_now_add=True, blank=True, verbose_name='投稿日')

    def __str__(self):
        # 管理画面の一覧にタイトルを表示できるように設定
        return self.title

    class Meta:
        # 属性と属性値を参照してそのモデルのオブジェクトの扱い方を変更する
        # 管理サイト上のモデルの表記方法を変更する
        verbose_name_plural = "記事"
```

## 4. settings.pyの編集
**アプリケーションの起動**が出来るようアプリケーション側のapps.pyのクラスを追加します。
1. `INSTALLED_APPSの配列`に`'Study.apps.StudyConfig',`を追加します。
```html
<body>
	{% for article in object_list %}
	<li>{{ article.title }}</li>
	{% endfor %}
</body>
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
🥟を焼くときに、綺麗に一列に並べたくなりますね。
