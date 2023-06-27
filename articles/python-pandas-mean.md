---
title: "【Python】pandasで平均値を求める" # 記事のタイトル
emoji: "⚖" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア記事
topics: ["python", "pandas"] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開設定（falseにすると下書き）
---
## はじめに
Pythonのデータ分析の学習を始めたいと思い、
Python2年生 データ分析のしくみ 体験してわかる！会話でまなべる！を購入しました。
そこで`pandas`の使い方を学びました。

学習した内容を執筆します。

|  項目  | 内容  |
| ---- | ---- |
|  **対象者**  |  ・pandasで平均値を求める方法を知りたい方<br>・Python微経験者  |
|  **伝えたい内容**  |  ・pandasで平均値を求める方法  |
|  **前提条件**  |  ・Python 3.9.10<br>・pandas 1.5.2 |


## 平均値を求めるサンプルコード
### 1. pandasインストール
下記コマンドでインストールします。
```bash
pip install pandas
```
下記コマンドでインストールされたか確認します。
```bash
$ pip show pandas
Name: pandas
Version: 1.5.2
Summary: Powerful data structures for data analysis, time series, and statisticsHome-page: https://pandas.pydata.org
Author: The Pandas Development Team
Author-email: pandas-dev@python.org
License: BSD-3-Clause
Location: /home/furuta/.local/lib/python3.9/site-packages
Requires: numpy, python-dateutil, pytz
Required-by:
```
### 2. pandas.mean()でコーディングする
```python: python-mean.py
import pandas as pd

data = {
    "東京の気温(2020年)": [7.1, 8.3, 10.7, 12.8, 19.5, 23.2, 24.3, 29.1, 24.2, 17.5, 14.0, 7.7],
    "大阪の気温(2020年)": [8.6, 8.0, 11.4, 13.7,20.8, 24.9, 26.0, 30.7, 25.8, 18.7, 14.7, 8.7]
}
df = pd.DataFrame(data)
# 全てのデータを出力する
print(df.mean())
# 個別のデータを出力する
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

## おわりに
平均値を求める＝バランスを取るイメージで"⚖"を選びました。

