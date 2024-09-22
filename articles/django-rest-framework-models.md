---
title: '[Django]modelsのField一覧' # 記事のタイトル
emoji: '🥕' # アイキャッチとして使われる絵文字（1文字だけ）
type: 'tech' # tech: 技術記事 / idea: アイデア記事
topics: ['python', 'django', '初心者向け'] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開設定（falseにすると下書き）
---

## はじめに

社内の有志メンバーの活動で Django を利用するので、models について理解する必要があったので、
下記の公式ドキュメントを読み、本記事にまとめます。
@[card](https://docs.djangoproject.com/ja/4.2/topics/db/models/)

## Field(フィールド)一覧

| Field の種類            | フィールドの説明                                         |
| ----------------------- | -------------------------------------------------------- | ------------------ |
| `BooleanField`          | boolean 値 (True/False)                                  |
| `CharField`             | 文字列                                                   |
| `TextField`             | 長い文字列（テキスト）                                   | `max_length`が必須 |
| `SlugField`             | 文字列(アルファベット、数字、アンダーバー、ハイフンのみ) |
| `JSONField`             | JSON エンコードされたデータ                              |
| `IntegerField`          | 整数                                                     |
| `FloatField`            | 浮動小数点数                                             |
| `PositiveIntegerField`  | 0 または正の整数                                         |
| `DateTimeField`         | 日付と時刻                                               |
| `DateField`             | 日付                                                     |
| `TimeField`             | 時刻                                                     |
| `EmailField`            | メールアドレス                                           |
| `URLField`              | URL                                                      |
| `FileField`             | ファイル                                                 |
| `ImageField`            | 画像ファイル                                             |
| `GenericIPAddressField` | IP アドレス                                              |

## Field 詳細

### BooleanField

```python
# 初期値をNoneにする場合
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
# 最大文字数にしたい場合(最大文字数:255)
hoge = models.CharField(max_length=255)
```

:::message
【注意】`max_length`の設定は必須です
指定しない場合、以下のエラーが発生します。

```bash
CharFields must define a 'max_length' attribute.
```

:::

---

### TextField

```python
hoge = models.TextField()
```

:::message
【注意】長い文字列（テキスト）を扱う場合はこちらを使用する
:::

---

### SlugField

```python
# max_length が指定されていないとき、最大文字数は50になる
hoge = models.SlugField()
```

:::message
【注意】`アルファベット`、`数字`、`アンダーバー`、`ハイフン`以外の文字の場合は、エラー発生
:::

---

### JSONField

`JSONField`は、MariaDB, MySQL, Oracle, PostgreSQL, SQLite のみにサポートされている
※SQLite は、JSON1 extension が有効状態のみ

```python
# SON エンコードされた文字列を出力される
hoge = models.JSONField()
```

---

### IntegerField

`-2147483648`から`2147483647`までの値は、Django でサポートされているすべてのデータベースで安全です。

```python
from django.core.validators import MaxValueValidator, MinValueValidator
# 最大数を1000/最小数を1にする場合
hoge = models.IntegerField(validators=[MinValueValidator(1), MaxValueValidator(1000)])
```

---

### FloatField

`localize`が`False`のとき`NumberInput`で、そうでなければ`TextInput`となります。

```python
from django.core.validators import MaxValueValidator, MinValueValidator
# 最大数を0.999/最小数を0.001にする場合
hoge = models.FloatField(validators=[MinValueValidator(0.00), MaxValueValidator(0.999)])
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
# 対象のオブジェクトが最初に作成されたときに自動的に現在の日付/時間が保存する
created_at = models.DateTimeField(auto_now_add=True)
```

```python
# 対象のオブジェクトが変更される度に自動的に現在の日付/時間が保存する
updated_at = models.DateTimeField(auto_now=True)
```

※オプション`auto_now`と`auto_now_add`はデフォルトが False です。

---

### DateField

```python
# 対象のオブジェクトが最初に作成されたときに自動的に現在の日付が保存する
created_at = models.DateField(auto_now_add=True)
```

```python
# 対象のオブジェクトが変更される度に自動的に現在の日付が保存する
updated_at = models.DateField(auto_now=True)
```

※オプション`auto_now`と`auto_now_add`はデフォルトが False です。

---

### TimeField

```python
# 対象のオブジェクトが最初に作成されたときに自動的に現在の時間が保存する
created_at = models.TimeField(auto_now_add=True)
```

```python
# 対象のオブジェクトが変更される度に自動的に現在の時間が保存する
updated_at = models.TimeField(auto_now=True)
```

※オプション`auto_now`と`auto_now_add`はデフォルトが False です。

---

### EmailField

```python
hoge = models.EmailField()
```

:::message
【注意】入力された値に`@`がないとエラーが発生する
:::

---

### URLField

```python
hoge = models.URLField()
```

※最大文字数はデフォルトで 200 文字
:::message
【注意】デフォルトで入力された値に`http`,`https`, `ftp`,`ftps`ないとエラーが発生する
:::

---

### FileField

```python
# upload_to: MEDIA_ROOT配下の指定したファイルパスに保存される
# FileExtensionValidator: 登録できる拡張子を指定できる
hoge = models.FileField(upload_to='uploads/%Y/%m/%d/', validators=[FileExtensionValidator(['pdf'])])
```

---

### ImageField

```python
# upload_to: MEDIA_ROOT配下の指定したファイルパスに保存される
# FileExtensionValidator: 登録できる拡張子を指定できる
hoge = models.ImageField(upload_to='uploads/%Y/%m/%d/', validators=[FileExtensionValidator(['png'])])
```

---

### GenericIPAddressField

```python
hoge = models.GenericIPAddressField()
```

> Pv4 か IPv6 のアドレスで、文字列フォーマットです (例: 192.0.2.30 ないし 2a02:42fe::4)。このフィールドのデフォルトのフォームウィジェットは TextInput です。<br><br>IPv6 アドレスは、 RFC 4291#section-2.2 section 2.2 (同セクションの paragraph 3 で提案された IPv4 のフォーマットの使用を含む) にしたがって、 ::ffff:192.0.2.0 のように正規化します。たとえば、 2001:0::0:01 は 2001::1 と正規化され、 ::ffff:0a0a:0a0a は ::ffff:10.10.10.10 と正規化されます。そして、すべての文字は小文字に変換されます。

## Field options(フィールドオプション)一覧

| Field options の種類 | フィールドの説明               |
| -------------------- | ------------------------------ |
| `null`               | null の許容                    |
| `blank`              | blank の許容                   |
| `choices`            | 任意の選択肢                   |
| `default`            | デフォルト値の設定             |
| `primary_key`        | プライマリーキーの設定         |
| `unique`             | 一意制約の設定                 |
| `verbose_name`       | 管理画面でのモデルの名前を指定 |
| `validators`         | バリデータの設定               |

---

### null

```python
# DBにnullを保存できるようにする
hoge = models.BooleanField(null=True)
```

```python
# DBにnullを保存出来ないようにする
hoge = models.BooleanField(null=False)
```

---

### blank

```python
# DBへの入力を必須にする
hoge = models.BooleanField(blank=True)
```

```python
# DBに空でもOkにする
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

`primary_key=True`が設定されなかった場合、Django は自動的に主キーを保存するために`IntegerField`を追加

```python
# idではなく、nameを主キーにしたい場合
name = models.CharField(max_length=100, primary_key=True)
```

---

### unique

社員番号など重複が許されていないデータ

```python
hoge = models.PositiveIntegerField(unique=True)
```

---

### verbose_name

管理画面で`title`ではなく、`タイトル`と表示してくれる

```python
title = models.CharField(max_length=, verbose_name='タイトル')
```

---

### validators

```python
from django.core.validators import MaxValueValidator, MinValueValidator
# 最大数を1000/最小数を1にする場合
hoge = models.IntegerField(validators=[MinValueValidator(1), MaxValueValidator(1000)])
```

## おわりに

今回公式ドキュメントを読み、「こんな事まで出来るのか！」と色々学びになりました。
やぱり、公式ドキュメントを読んで理解できるエンジニアは強いなと感じました。
