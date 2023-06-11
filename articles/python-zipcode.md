---
title: "【Python】APIで郵便番号から住所を取得する方法" # 記事のタイトル
emoji: "🥝" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア記事
topics: ["python", ] # タグ。["markdown", "rust", "aws"]のように指定する
published: false # 公開設定（falseにすると下書き）
---
# はじめに
## 前提条件
- Python 3.9.10
- requests 2.28.2
### requestsのインストール方法
1. 下記コマンドでインストールする
```Git bash
pip install requests
```
2. 
```Git bash
pip show requests
```
## 手順解説
### ライブラリーのインストール
1. requestsライブラリーをインストールするために、ターミナル上で`pip install requests`を実行します。
```git
pip install requests
```
2. ターミナル上で`pip show requests`を実行し、requestsライブラリーがインストールされているかを確認します。
```git
pip show requests
```
requests 2.28.2がインストールされたことが確認出来ました。
```git
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
### コーディング
1.使用するライブラリーをインポートする
使用するファイル内の上段で記載し、インポートします。
pprintは、出力するjsonを見やすくするためにインポートします。
```python
from pprint import pprint
import requests
```

## サンプルコード解説
```python
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

```json
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