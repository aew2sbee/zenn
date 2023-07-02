---
title: "【Python】matplotlibで円グラフを描く" # 記事のタイトル
emoji: "🍥" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア記事
topics: ["python", "matplotlib"] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開設定（falseにすると下書き）
---
## はじめに
Pythonのデータ分析の学習を始めたいと思い、
Python2年生 データ分析のしくみ 体験してわかる！会話でまなべる！を購入しました。
そこでmatplotlibで円グラフを描くを学びました。

学習した内容を執筆します。

|  項目  | 内容  |
| ---- | ---- |
|  **対象者**  |  ・matplotlibで円グラフを描く方法が分からない  |
|  **伝えたい内容**  |  ・Pythonで円グラフを描く方法  |
|  **前提条件**  |  ・Python 3.9.10<br>・matplotlib 3.6.2<br>・seaborn 0.12.2<br>・pandas 1.5.2 |


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
円グラフを描きます
```python
import matplotlib.pyplot as plt
import pandas as pd
import seaborn as sns

# グラフにstyleとfontを設定する
sns.set(style="whitegrid", font=["Yu Gothic"])

data = {
    "つぶあん": [62, 47, 18],
    "こしあん": [35, 26, 12]
}
index = ["好き", "普通", "嫌い"]
df = pd.DataFrame(data, index)

# 円グラフを表示する
# startangle=90, counterclock=False データの始まりを真上から開始するオプション
# labeldistance=0.5 データのindexを内側に入れるオプション
df["つぶあん"].plot.pie(startangle=90, counterclock=False, labeldistance=0.5)
plt.show()
```
1. VScodeで作業中のファイルを開き、そのファイル上で**右クリック**を押してください。
2. 下記画像のように候補の中に**ターミナルでPythonファイルを実行する**をクリックする
![ターミナルでPythonファイルを実行する](/images/run-matplotlib-vscode.png)
期待通りのグラフが画像として表示されました！
![pie_sample](/images/pie_sample.png)

## おわりに
🍥はぐるぐる

