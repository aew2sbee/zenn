---
title: "[Python] APIで郵便番号から住所を取得する方法" # 記事のタイトル
emoji: "🐍" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア記事
topics: ["python", "webapi"] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開:true / 非公開:false
---

## 🌱 はじめに

先日、**API で郵便番号から住所を取得する**方法を学習しました。
自分の理解をより深めるために執筆します。

また、ユーザー登録時の住所入力の処理などで使えると思います。

| 項目             | 内容                                                 |
| ---------------- | ---------------------------------------------------- |
| **対象者**       | ・API で郵便番号から住所を取得する方法が分からない方 |
| **伝えたい内容** | ・API で郵便番号から住所を取得する方法               |
| **前提条件**     | ・Python 3.9.10<br>・requests 2.28.2                 |

## 🌱 手順解説

### 1. requests ライブラリをインストールする

下記コマンドでインストールします。

```bash
pip install requests
```

### 2. requests ライブラリを確認する

`Version: 2.28.2`がインストールされていることが確認できます。

```bash
$ pip show requests
Name: requests
Version: 2.28.2
Summary: Python HTTP for Humans.
Home-page: https://requests.readthedocs.io
Author: Kenneth Reitz
Author-email: me@kennethreitz.org
License: Apache 2.0
Location: /home/user/.local/lib/python3.9/site-packages
Requires: certifi, charset-normalizer, idna, urllib3
Required-by:
```

:::message alert
requests 2.28.2 は執筆時点のバージョンです。このバージョンには既知の脆弱性があるため、現在は`pip install requests`で入る最新版を使ってください。
:::

### 3. 使用するライブラリをインポートする

ファイルの先頭に記載し、インポートします。

:::message
pprint は、出力するデータを見やすくするためにインポートします。
:::

```python
from pprint import pprint
import requests
```

### 4. API で住所の情報を取得する

今回は、**東京スカイツリーの住所**を取得することを目標とします。

> 〒 131-0045 東京都墨田区押上１丁目１ − ２

API で住所の情報を取得するには、**郵便番号、郵便番号検索 API の URL**が必要です。
`zipcode`と`URL`を変数として用意します。
先ほどの変数を`requests.get()`の引数として渡します。

```python
# スカイツリーの郵便番号
zipcode = "1310045"
# 郵便番号検索APIのURLを定数化する
URL = 'https://zipcloud.ibsnet.co.jp/api/search'
# paramsで検索したい郵便番号を渡す
res = requests.get(URL, params={'zipcode': zipcode})
```

### 5. API で取得した情報を出力する

返ってきた JSON 形式のデータを、`res.json()`で Python の辞書に変換して出力します。

```python
# 検索した住所を出力する
pprint(res.json())
```

## 🌱 サンプルコード

先ほどの手順解説をまとめたコードになります。

```python:zipcode.py
# 取得したデータを見やすく出力するためにpprintを使う
from pprint import pprint
# ライブラリをインポートする
import requests

# スカイツリーの郵便番号
zipcode = "1310045"
# 郵便番号検索APIのURLを定数化する
URL = 'https://zipcloud.ibsnet.co.jp/api/search'
# paramsで検索したい郵便番号を渡す
res = requests.get(URL, params={'zipcode': zipcode})

# 検索した住所を出力する
pprint(res.json())
```

address1~3 を組み合わせたら、
`東京都墨田区押上`になり目的の東京スカイツリーの住所を取得することができました。

```bash
$ python zipcode.py
{'message': None,
 'results': [{'address1': '東京都',
              'address2': '墨田区',
              'address3': '押上',
              'kana1': 'ﾄｳｷｮｳﾄ',
              'kana2': 'ｽﾐﾀﾞｸ',
              'kana3': 'ｵｼｱｹﾞ',
              'prefcode': '13',
              'zipcode': '1310045'}],
 'status': 200}
```
