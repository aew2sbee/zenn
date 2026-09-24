---
title: "[Python] matplotlibで直線グラフを描く" # 記事のタイトル
emoji: "🐍" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア記事
topics: ["python", "matplotlib", "初心者向け"] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開:true / 非公開:false
---

## 🌱 はじめに

Python のデータ分析の学習を始めたいと思い、
森 巧尚『Python2年生 データ分析のしくみ 体験してわかる！会話でまなべる！』（翔泳社）を購入しました。

https://www.shoeisha.co.jp/book/detail/9784798164960

そこで、`matplotlib`で直線グラフを描く方法を学びました。

学習した内容をまとめます。

|項目|内容|
|---|---|
|**対象者**|・matplotlib でグラフを描く方法が分からない|
|**伝えたい内容**|・Python でグラフを描く方法|
|**前提条件**|・Python 3.9.10<br>・matplotlib 3.6.2<br>・VS Code + Python 拡張機能|

## 🌱 サンプルコード

### 1. ライブラリをインストール

```bash
pip install matplotlib
```

:::message
VS Code で実行する場合は、VS Code で選択している Python（インタープリター）に matplotlib をインストールしてください。
:::

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

### 2. コーディング

右肩上がりの直線（傾き1、y = x + 1）のグラフを描きます。

```python
import matplotlib.pyplot as plt

# グラフとして描画するデータ
# x軸とy軸の値をリストで設定する
x = [1,2,3,4]
y = [2,3,4,5]

# x軸とy軸の値を引数にプロットする
plt.plot(x, y)
plt.show()
```

1. VS Code で作業中のファイルを開き、そのファイル上で**右クリック**してください。
2. 下記画像のように、候補の中から**ターミナルで Python ファイルを実行する**をクリックしてください。

   ![ターミナルでPythonファイルを実行する](/images/articles/python-matplotlib-45graph/run-matplotlib-vscode.png)

期待通りのグラフがウィンドウに表示されました！（ウィンドウを閉じるとスクリプトが終了します）

![右肩上がりの直線のグラフ](/images/articles/python-matplotlib-45graph/45graph.png)

:::message
グラフの見た目の角度は、軸の表示範囲や縦横比によって変わります。
GUI が使えない環境でグラフが表示されない場合は、`plt.savefig('graph.png')` で画像として保存できます。
:::
