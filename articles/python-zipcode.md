---
title: "【Python】APIで郵便番号から住所を取得する方法" # 記事のタイトル
emoji: "📮" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア記事
topics: ["python", "webapi"] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開設定（falseにすると下書き）
---
## はじめに
先日、**APIで郵便番号から住所を取得する**方法を学習しました。
より自分の理解が深めるために執筆します。

また、ユーザー登録の住所登録の処理等で使えると思います。

### 対象読者
- APIで郵便番号から住所を取得する方法が分からない方

### この記事でわかること
- APIで郵便番号から住所を取得する方法が分かる


### 前提条件
- Python 3.9.10
- requests 2.28.2

## 手順解説
### 1. requestsライブラリーをインストールする
下記コマンドでインストールする
```bash
pip install requests
```
### 2. requestsライブラリーを確認する
`Version: 2.28.2`がインストールされている事が確認できます。
```bash
$ pip show requests
Name: requests
Version: 2.28.2
Summary: Python HTTP for Humans.
Home-page: https://requests.readthedocs.io
Author: Kenneth Reitz
Author-email: me@kennethreitz.org
License: Apache 2.0
Location: /home/furuta/.local/lib/python3.9/site-packages
Requires: certifi, charset-normalizer, idna, urllib3
Required-by:
```

### 3. 使用するライブラリーをインポートする
使用するファイル内の上段で記載し、インポートします。
:::message
pprintは、出力するjsonを見やすくするためにインポートします。
:::
```python
from pprint import pprint
import requests
```

### 4. APIで住所の情報を取得する
今回は、**東京スカイツリーの住所**を取得する事を目標とします
> 〒131-0045 東京都墨田区押上１丁目１−２

APIで住所の情報を取得するには、**郵便番号、郵便番号検索APIのURL**が必要です。
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

### 4. APIで取得した情報を出力する
返ってきたデータを`res.json()`でjson形式に変換する
```python
# 検索した住所を出力する
pprint(res.json())
```
## サンプルコード
先ほどの手順解説をまとめたコードになります。
```python:zipcode.py
# jsonの中身を見やすいようにpprintを活用
from pprint import pprint
# ライブラリーをインポートする
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

address1~3を組み合わせたら、
`東京都墨田区押上`になり目的の東京スカイツリーの住所を取得する事が出来ました。
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

## おわりに
requestsライブラリーを初めて触りましたが
今回の学習でrequestsライブラリーの使い方を理解する事が出来ました。

他にもAPIもあるので、色々触って遊びたいと思います。

