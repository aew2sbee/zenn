---
title: "[Python] pandasで平均値を求める" # 記事のタイトル
emoji: "🐍" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア記事
topics: ["python", "pandas"] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開:true / 非公開:false
---

## 🌱 はじめに

Python のデータ分析の学習を始めたいと思い、
森 巧尚『Python2年生 データ分析のしくみ 体験してわかる！会話でまなべる！』（翔泳社）を購入しました。
そこで、`pandas`の使い方を学びました。

学習した内容をまとめます。

| 項目             | 内容                                                           |
| ---------------- | -------------------------------------------------------------- |
| **対象者**       | ・pandas で平均値を求める方法を知りたい方<br>・Python を少し触ったことがある方 |
| **伝えたい内容** | ・pandas で平均値を求める方法                                  |
| **前提条件**     | ・Python 3.9.10<br>・pandas 1.5.2                              |

## 🌱 平均値を求めるサンプルコード

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

### 2. mean()でコーディングする

```python:python-mean.py
import pandas as pd

data = {
    "東京の気温(2020年)": [7.1, 8.3, 10.7, 12.8, 19.5, 23.2, 24.3, 29.1, 24.2, 17.5, 14.0, 7.7],
    "大阪の気温(2020年)": [8.6, 8.0, 11.4, 13.7, 20.8, 24.9, 26.0, 30.7, 25.8, 18.7, 14.7, 8.7]
}
df = pd.DataFrame(data)
# 列ごとの平均値をまとめて出力する
print(df.mean())
# 列を指定して平均値を出力する
print("東京の気温(2020年) =", df["東京の気温(2020年)"].mean())
print("大阪の気温(2020年) =", df["大阪の気温(2020年)"].mean())
```

### 3. 出力結果を確認する

```bash
$ python python-mean.py
東京の気温(2020年)    16.533333
大阪の気温(2020年)    17.666667
dtype: float64
東京の気温(2020年) = 16.53333333333333
大阪の気温(2020年) = 17.666666666666668
```
