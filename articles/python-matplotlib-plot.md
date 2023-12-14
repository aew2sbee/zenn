---
title: '[Python] matplotlibで折れ線グラフを描く' # 記事のタイトル
emoji: '⛺' # アイキャッチとして使われる絵文字（1文字だけ）
type: 'tech' # tech: 技術記事 / idea: アイデア記事
topics: ['python', 'matplotlib', '初心者向け'] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開設定（falseにすると下書き）
---

## はじめに

Python のデータ分析の学習を始めたいと思い、
Python2 年生 データ分析のしくみ 体験してわかる！会話でまなべる！を購入しました。
そこで matplotlib で折れ線グラフを描くを学びました。

学習した内容を執筆します。

| 項目             | 内容                                                                        |
| ---------------- | --------------------------------------------------------------------------- |
| **対象者**       | ・matplotlib で折れ線グラフを描く方法が分からない                           |
| **伝えたい内容** | ・Python で折れ線グラフを描く方法                                           |
| **前提条件**     | ・Python 3.9.10<br>・matplotlib 3.6.2<br>・seaborn 0.12.2<br>・pandas 1.5.2 |

## サンプルコード

### 1. ライブラリーをインストール

```bash
pip install matplotlib
pip install pandas
pip install seaborn
```

インストールが出来たか下記コマンドで確認します。

```bash
$ pip show matplotlib
Name: matplotlib
Version: 3.6.2
Summary: Python plotting package
Home-page: https://matplotlib.org
Author: John D. Hunter, Michael Droettboom
Author-email: matplotlib-users@python.org
License: PSF
Location: /home/furuta/.local/lib/python3.9/site-packages
Requires: contourpy, cycler, fonttools, kiwisolver, numpy, packaging, pillow, pyparsing, python-dateutil
Required-by:
```

```bash
$ pip show seaborn
Name: seaborn
Version: 0.12.2
Summary: Statistical data visualization
Home-page:
Author:
Author-email: Michael Waskom <mwaskom@gmail.com>
License:
Location: /home/furuta/.local/lib/python3.9/site-packages
Requires: matplotlib, numpy, pandas
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
Location: /home/furuta/.local/lib/python3.9/site-packages
Requires: numpy, python-dateutil, pytz
Required-by:
```

### 2. コーディング

折れ線グラフを描きます

```python
import matplotlib.pyplot as plt
import pandas as pd
import seaborn as sns

# グラフにstyleとfontを設定する
sns.set(style="whitegrid", font=["Yu Gothic"])

data = {
    "東京の気温(2020年)": [7.1, 8.3, 10.7, 12.8, 19.5, 23.2, 24.3, 29.1, 24.2, 17.5, 14.0, 7.7],
    "大阪の気温(2020年)": [8.6, 8.0, 11.4, 13.7, 20.8, 24.9, 26.0, 30.7, 25.8, 18.7, 14.7, 8.7]
}
df = pd.DataFrame(data)

# 折れ線グラフを表示する
df.plot()
plt.show()
```

1. VScode で作業中のファイルを開き、そのファイル上で**右クリック**を押してください。
2. 下記画像のように候補の中に**ターミナルで Python ファイルを実行する**をクリックする
   ![ターミナルでPythonファイルを実行する](/images/run-matplotlib-vscode.png)
   期待通りのグラフが画像として表示されました！
   ![plot](/images/plot.png)

## おわりに

今回のグラフは ⛺ に見える
