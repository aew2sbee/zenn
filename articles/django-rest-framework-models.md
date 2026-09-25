---
title: "[Django] modelsのField一覧" # 記事のタイトル
emoji: "🚀" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア記事
topics: ["python", "django", "初心者向け"] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開:true / 非公開:false
---

## 🌱 はじめに

社内の有志メンバーの活動で Django を利用することになり、models について理解する必要があったため、下記の公式ドキュメントを読んで、よく使う Field と Field options を本記事にまとめます。
ForeignKey などのリレーション系のフィールドは扱いません。

@[card](https://docs.djangoproject.com/ja/5.2/topics/db/models/)

@[card](https://docs.djangoproject.com/ja/5.2/ref/models/fields/)

## 🌱 models の基本

Django の model は、データベースのテーブルに対応する Python のクラスです。Field は、テーブルの各列（カラム）の型を表します。
本記事のコード例は、下記のように`models.py`のモデルクラスの中に書く前提です。`python manage.py makemigrations`と`python manage.py migrate`を実行すると、データベースに反映されます。

```python:models.py
from django.db import models

class Article(models.Model):
    title = models.CharField(max_length=100)
    body = models.TextField()
```

## 🌱 Field（フィールド）一覧

| Field の種類            | フィールドの説明                                         | 備考               |
| ----------------------- | -------------------------------------------------------- | ------------------ |
| `BooleanField`          | boolean 値（True/False）                                 |                    |
| `CharField`             | 文字列                                                   | `max_length`が必須（PostgreSQL・SQLite を除く） |
| `TextField`             | 長い文字列（テキスト）                                   |                    |
| `SlugField`             | 文字列（ASCII のアルファベット、数字、アンダーバー、ハイフンのみ） |                    |
| `JSONField`             | JSON エンコードされたデータ                              |                    |
| `IntegerField`          | 整数                                                     |                    |
| `FloatField`            | 浮動小数点数                                             |                    |
| `PositiveIntegerField`  | 0 または正の整数                                         |                    |
| `DateTimeField`         | 日付と時刻                                               |                    |
| `DateField`             | 日付                                                     |                    |
| `TimeField`             | 時刻                                                     |                    |
| `EmailField`            | メールアドレス                                           |                    |
| `URLField`              | URL                                                      |                    |
| `FileField`             | ファイル                                                 |                    |
| `ImageField`            | 画像ファイル                                             |                    |
| `GenericIPAddressField` | IP アドレス                                              |                    |

:::message
本記事で「エラーが発生する」と書いている入力値の検証（バリデーション）は、フォームや`full_clean()`を通したときに実行されます。`save()`を直接呼んだ場合は実行されません。
:::

## 🌱 Field 詳細

### BooleanField

```python
# default を指定しない場合、初期値は None になる
# ※null=False（デフォルト）のまま値を入れずに保存するとエラーになる
hoge = models.BooleanField()
```

```python
# 初期値をTrueにする場合
hoge = models.BooleanField(default=True)
```

```python
# 初期値をFalseにしたい場合
hoge = models.BooleanField(default=False)
```

---

### CharField

```python
# 最大文字数を255にしたい場合
hoge = models.CharField(max_length=255)
```

:::message
【注意】PostgreSQL・SQLite 以外のデータベースでは、`max_length`の設定は必須です。
指定しない場合、`python manage.py check`などで以下のエラーが発生します。

```text
CharFields must define a 'max_length' attribute.
```

:::

---

### TextField

長い文字列（テキスト）を扱う場合に使用します。`CharField`と異なり、`max_length`は必須ではありません。

```python
hoge = models.TextField()
```

---

### SlugField

```python
# max_length が指定されていないとき、最大文字数は50になる
hoge = models.SlugField()
```

:::message
【注意】ASCII の`アルファベット`、`数字`、`アンダーバー`、`ハイフン`以外の文字が含まれると、エラーが発生します（`allow_unicode=True`で Unicode 文字も許可できます）。
:::

---

### JSONField

`JSONField`は、MariaDB、MySQL、Oracle、PostgreSQL、SQLite でのみサポートされています。
※SQLite は、JSON1 extension が有効な場合のみ

```python
# dict や list など、JSON にシリアライズ可能な Python の値を保存・取得できる
hoge = models.JSONField()
```

---

### IntegerField

`-2147483648`から`2147483647`（32 ビット整数の範囲）までの値は、Django でサポートされているすべてのデータベースで安全です。

```python
from django.core.validators import MaxValueValidator, MinValueValidator
# 最大数を1000/最小数を1にする場合
hoge = models.IntegerField(validators=[MinValueValidator(1), MaxValueValidator(1000)])
```

---

### FloatField

```python
from django.core.validators import MaxValueValidator, MinValueValidator
# 最大数を0.999/最小数を0.001にする場合
hoge = models.FloatField(validators=[MinValueValidator(0.001), MaxValueValidator(0.999)])
```

---

### PositiveIntegerField

`0`から`2147483647`までの値は、Django でサポートされているすべてのデータベースで安全です。

```python
from django.core.validators import MaxValueValidator, MinValueValidator
# 最大数を2147483647/最小数を1にする場合
hoge = models.PositiveIntegerField(validators=[MinValueValidator(1), MaxValueValidator(2147483647)])
```

---

### DateTimeField

```python
# 対象のオブジェクトが最初に作成されたときに、自動的に現在の日付/時刻が保存される
created_at = models.DateTimeField(auto_now_add=True)
```

```python
# 対象のオブジェクトが保存される度に、自動的に現在の日付/時刻が保存される
updated_at = models.DateTimeField(auto_now=True)
```

※オプション`auto_now`と`auto_now_add`はデフォルトが False です。`auto_now`は`save()`のときに更新され、`QuerySet.update()`では更新されません。

---

### DateField

`DateTimeField`と同じく、`auto_now`と`auto_now_add`が使えます。

```python
# 対象のオブジェクトが最初に作成されたときに、自動的に現在の日付が保存される
created_at = models.DateField(auto_now_add=True)
```

```python
# 対象のオブジェクトが保存される度に、自動的に現在の日付が保存される
updated_at = models.DateField(auto_now=True)
```

---

### TimeField

`DateTimeField`と同じく、`auto_now`と`auto_now_add`が使えます。

```python
# 対象のオブジェクトが最初に作成されたときに、自動的に現在の時刻が保存される
created_at = models.TimeField(auto_now_add=True)
```

```python
# 対象のオブジェクトが保存される度に、自動的に現在の時刻が保存される
updated_at = models.TimeField(auto_now=True)
```

---

### EmailField

```python
hoge = models.EmailField()
```

:::message
【注意】`EmailValidator`でメールアドレスの形式が検証され、形式が正しくないとエラーが発生します。デフォルトの最大文字数は 254 文字です。
:::

---

### URLField

```python
hoge = models.URLField()
```

※最大文字数はデフォルトで 200 文字

:::message
【注意】デフォルトでは、入力値が`http`、`https`、`ftp`、`ftps`のいずれかで始まらないとエラーが発生します。
:::

---

### FileField

```python
from django.core.validators import FileExtensionValidator
# upload_to: MEDIA_ROOT配下の指定したパスに保存される（%Y/%m/%d はアップロード日の年/月/日）
# FileExtensionValidator: 登録できる拡張子を指定できる
hoge = models.FileField(upload_to='uploads/%Y/%m/%d/', validators=[FileExtensionValidator(['pdf'])])
```

※`MEDIA_ROOT`は、`settings.py`で指定するアップロード先のディレクトリです（デフォルトのストレージを使う場合）。

---

### ImageField

```python
from django.core.validators import FileExtensionValidator
# upload_to: MEDIA_ROOT配下の指定したパスに保存される（%Y/%m/%d はアップロード日の年/月/日）
# FileExtensionValidator: 登録できる拡張子を指定できる
hoge = models.ImageField(upload_to='uploads/%Y/%m/%d/', validators=[FileExtensionValidator(['png'])])
```

:::message
【注意】`ImageField`を使うには、`pip install Pillow`で Pillow をインストールする必要があります。
:::

---

### GenericIPAddressField

IPv4 または IPv6 のアドレスを文字列で保存します（例: `192.0.2.30`、`2a02:42fe::4`）。IPv6 のアドレスは、小文字化などの正規化が行われます。

```python
hoge = models.GenericIPAddressField()
```

@[card](https://docs.djangoproject.com/ja/5.2/ref/models/fields/#genericipaddressfield)

## 🌱 Field options（フィールドオプション）一覧

| Field options の種類 | フィールドの説明                                  |
| -------------------- | ------------------------------------------------- |
| `null`               | DB に null を保存できるか                         |
| `blank`              | フォームなどの入力チェックで空を許可するか        |
| `choices`            | 任意の選択肢                                      |
| `default`            | デフォルト値の設定                                |
| `primary_key`        | プライマリーキー（主キー）の設定                  |
| `unique`             | 一意制約の設定                                    |
| `verbose_name`       | フィールドの表示名（管理画面やフォームのラベル）  |
| `validators`         | バリデータの設定                                  |

---

### null

```python
# DBにnullを保存できるようにする
hoge = models.BooleanField(null=True)
```

```python
# DBにnullを保存できないようにする（デフォルト）
hoge = models.BooleanField(null=False)
```

---

### blank

`null`が DB に保存できるかを決めるのに対し、`blank`はフォームなどの入力チェック（バリデーション）で空を許可するかを決めます。

```python
# 入力チェックで空を許可する
hoge = models.BooleanField(blank=True)
```

```python
# 入力チェックで入力を必須にする（デフォルト）
hoge = models.BooleanField(blank=False)
```

---

### choices

```python
# TシャツのサイズをSMLのいずれかにする
SHIRT_SIZES = [
    ("S", "Small"),
    ("M", "Medium"),
    ("L", "Large"),
]
shirt_size = models.CharField(max_length=1, choices=SHIRT_SIZES)
```

---

### default

```python
# データの指定がない場合TrueがDBに保存される
hoge = models.BooleanField(default=True)
```

---

### primary_key

主キーは、各レコードを一意に識別するための列です。
`primary_key=True`を指定しない場合、Django は自動採番の主キー`id`を追加します。型は`DEFAULT_AUTO_FIELD`で指定したもの（新規プロジェクトのデフォルトは`BigAutoField`）です。

```python
# idではなく、nameを主キーにしたい場合
name = models.CharField(max_length=100, primary_key=True)
```

---

### unique

社員番号など、重複を許さないデータに使います。

```python
hoge = models.PositiveIntegerField(unique=True)
```

---

### verbose_name

管理画面などで、`title`ではなく`タイトル`と表示してくれます。

```python
title = models.CharField(max_length=100, verbose_name='タイトル')
```

---

### validators

値の範囲などを検証するバリデータを設定します。使用例は、上記の`IntegerField`や`FloatField`を参照してください。

## 🌱 おわりに

本記事では、Django の models でよく使う Field と Field options をまとめました。
特に、`null`（DB に保存できるか）と`blank`（入力チェックで空を許可するか）の違いは、混同しやすいので注意が必要です。
