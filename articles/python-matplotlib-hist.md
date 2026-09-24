---
title: "[Python] matplotlibでヒストグラムを描く" # 記事のタイトル
emoji: "🐍" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア記事
topics: ["python", "matplotlib", "初心者向け"] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開:true / 非公開:false
---

## 🌱 はじめに

Python のデータ分析の学習を始めたいと思い、
森 巧尚『Python2年生 データ分析のしくみ 体験してわかる！会話でまなべる！』（翔泳社）を購入しました。
そこで、matplotlib でヒストグラムを描く方法を学びました。

学習した内容をまとめます。

|項目|内容|
|---|---|
|**対象者**|・matplotlib でグラフを描く方法が分からない人|
|**伝えたい内容**|・Python でグラフを描く方法|
|**前提条件**|・Python 3.9.10<br>・matplotlib 3.6.2<br>・pandas 1.5.2<br>・VS Code + Python 拡張機能|

## 🌱 サンプルコード

### 1. ライブラリをインストール

```bash
pip install matplotlib
pip install pandas
```

インストールできたか下記コマンドで確認します。

```bash
$ pip show matplotlib
Name: matplotlib
Version: 3.6.2
Summary: Python plotting package
Home-page: https://matplotlib.org
Author: John D. Hunter, Michael Droettboom
Author-email: matplotlib-users@python.org
License: PSF
Location: /home/user/.local/lib/python3.9/site-packages
Requires: contourpy, cycler, fonttools, kiwisolver, numpy, packaging, pillow, pyparsing, python-dateutil
Required-by:
```

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

### 2. コーディング

ヒストグラムを描きます。

```python
import matplotlib.pyplot as plt
import pandas as pd

data = {
    "東京の気温(2020年)": [7.1, 8.3, 10.7, 12.8, 19.5, 23.2, 24.3, 29.1, 24.2, 17.5, 14.0, 7.7],
    "大阪の気温(2020年)": [8.6, 8.0, 11.4, 13.7, 20.8, 24.9, 26.0, 30.7, 25.8, 18.7, 14.7, 8.7]
}
df = pd.DataFrame(data)

# ヒストグラムを表示する
df["東京の気温(2020年)"].plot.hist(bins=[0, 5, 10, 15, 20, 25, 30])

# 日本語のタイトルが文字化けしないよう、フォントに MS Gothic を指定する
plt.title("東京の気温(2020年)", fontname="MS Gothic")
plt.show()
```

:::message
- `MS Gothic`は Windows 標準のフォントです。Linux や macOS では、IPAexGothic や Noto Sans CJK JP などの日本語フォントを指定してください。
- `fontname`はタイトルにだけ効きます。軸ラベルなどにも日本語を使う場合は、`plt.rcParams["font.family"] = "MS Gothic"`のようにグラフ全体のフォントを設定します。
:::

1. VS Code で作業中のファイルを開き、そのファイル上で**右クリック**してください。
2. 下記画像のように、候補の中から**ターミナルで Python ファイルを実行する**をクリックしてください。

   ![ターミナルでPythonファイルを実行する](/images/articles/python-matplotlib-hist/run-matplotlib-vscode.png)

期待通りのグラフがウィンドウに表示されました！（ウィンドウを閉じるとスクリプトが終了します）

![hist](/images/articles/python-matplotlib-hist/hist.png)
