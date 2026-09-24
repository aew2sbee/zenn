---
title: "[Python] pandasで範囲内のデータ数を求める" # 記事のタイトル
emoji: "🐍" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア記事
topics: ["python", "pandas"] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開:true / 非公開:false
---

## 🌱 はじめに

Python のデータ分析の学習を始めたいと思い、
森 巧尚『Python2年生 データ分析のしくみ 体験してわかる！会話でまなべる！』（翔泳社）を購入しました。

https://www.shoeisha.co.jp/book/detail/9784798164960

そこで、`pandas`の使い方を学びました。

学習した内容をまとめます。

| 項目             | 内容                                                                     |
| ---------------- | ------------------------------------------------------------------------ |
| **対象者**       | ・pandas で範囲内のデータ数を求める方法を知りたい方<br>・Python を少し触ったことがある方 |
| **伝えたい内容** | ・pandas で範囲内のデータ数を求める方法                                  |
| **前提条件**     | ・Python 3.9.10<br>・pandas 1.5.2                                        |

## 🌱 範囲内のデータ数を求めるサンプルコード

### 1. pandas インストール

下記コマンドでインストールします。

```bash
pip install pandas
```

下記コマンドでインストールされたか確認します。

```bash
$ pip show pandas
Name: pandas
Version: 1.5.2
Summary: Powerful data structures for data analysis, time series, and statistics
Home-page: https://pandas.pydata.org
Author: The Pandas Development Team
Author-email: pandas-dev@python.org
License: BSD-3-Clause
Location: /home/user/.local/lib/python3.9/site-packages
Requires: numpy, python-dateutil, pytz
Required-by:
```

### 2. value_counts()でコーディングする

```python:python-value_counts.py
import pandas as pd

data = {
    "東京の気温(2020年)": [7.1, 8.3, 10.7, 12.8, 19.5, 23.2, 24.3, 29.1, 24.2, 17.5, 14.0, 7.7],
    "大阪の気温(2020年)": [8.6, 8.0, 11.4, 13.7, 20.8, 24.9, 26.0, 30.7, 25.8, 18.7, 14.7, 8.7]
}
df = pd.DataFrame(data)
"""
right=False, bins = [0, 5, 10, 15, 20, 25, 30]の設定で
    0℃以上5℃未満, 5℃以上10℃未満, 10℃以上15℃未満
    15℃以上20℃未満, 20℃以上25℃未満, 25℃以上30℃未満の範囲を指定する
"""
bins = [0, 5, 10, 15, 20, 25, 30]
cut = pd.cut(df["東京の気温(2020年)"], bins=bins, right=False)
print(cut.value_counts(sort=False))

cut = pd.cut(df["大阪の気温(2020年)"], bins=bins, right=False)
print(cut.value_counts(sort=False))
```

### 3. 出力結果を確認する

```bash
$ python python-value_counts.py
[0, 5)      0
[5, 10)     3
[10, 15)    3
[15, 20)    2
[20, 25)    3
[25, 30)    1
Name: 東京の気温(2020年), dtype: int64
[0, 5)      0
[5, 10)     3
[10, 15)    3
[15, 20)    1
[20, 25)    2
[25, 30)    2
Name: 大阪の気温(2020年), dtype: int64
```

:::message
大阪の8月の気温（30.7℃）は、指定した範囲（30℃未満まで）の外にあるため集計されていません。そのため、大阪の件数の合計は11件になります。
:::
