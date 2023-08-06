---
title: "【Python】matplotlibで直線グラフを描く" # 記事のタイトル
emoji: "🎢" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア記事
topics: ["python", "matplotlib", "初心者向け"] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開設定（falseにすると下書き）
---
## はじめに
Pythonのデータ分析の学習を始めたいと思い、
Python2年生 データ分析のしくみ 体験してわかる！会話でまなべる！を購入しました。
そこでmatplotlibで直線グラフを描くを学びました。

学習した内容を執筆します。

|  項目  | 内容  |
| ---- | ---- |
|  **対象者**  |  ・matplotlibで直線グラフを描く方法が分からない  |
|  **伝えたい内容**  |  ・Pythonで直線グラフを描く方法  |
|  **前提条件**  |  ・Python 3.9.10<br>・matplotlib 3.6.2 |


## サンプルコード
### 1. ライブラリーをインストール
```bash
pip install matplotlib
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
### 2. コーディング
右斜め45度に伸びる直線のグラフを描きます
```python
import matplotlib.pyplot as plt

# グラフとして描画するデータ
# x軸とy軸の値を配列で設定する
x = [1,2,3,4]
y = [2,3,4,5]

# x軸とy軸の値を引数にプロットします
plt.plot(x, y)
plt.show()
```
1. VScodeで作業中のファイルを開き、そのファイル上で**右クリック**を押してください。
2. 下記画像のように候補の中に**ターミナルでPythonファイルを実行する**をクリックする
![ターミナルでPythonファイルを実行する](/images/run-matplotlib-vscode.png)
期待通りのグラフが画像として表示されました！
![右斜め45度に伸びる直線のグラフ](/images/45graph.png)

## おわりに
🎢の右斜め45度に上っている時って怖いですよね

