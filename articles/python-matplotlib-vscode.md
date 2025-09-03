---
title: '[Python] VScodeでmatplotlibのグラフを表示する方法' # 記事のタイトル
emoji: '🐍' # アイキャッチとして使われる絵文字（1文字だけ）
type: 'tech' # tech: 技術記事 / idea: アイデア記事
topics: ['python', 'matplotlib', 'vscode', 'データ分析'] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開設定（falseにすると下書き）
---

## 🌱 はじめに

Python のデータ分析の学習を始めたいと思い、
**Python2 年生 データ分析のしくみ 体験してわかる、会話でまなべる** を購入しました。

本書では、**Jupyter Notebook** を利用して学習を進めており、
VScode 上での学習方法の記載はありませんでした。

さらに、VScode からグラフを表示する方法がなく苦戦しました。
**他の方も苦戦しているのではないか**と思い、記録として残します。

## 🌱 結論

:::message
下記画像のように候補の中に**ターミナルで Python ファイルを実行する**をクリックする。
![ターミナルでPythonファイルを実行する](/images/articles/python-matplotlib-vscode/run-matplotlib-vscode.png)

:::

### 前提条件

- Python 3.9.10
- Visual Studio Code
- matplotlib 3.6.2

## 🌱 解説

### 1. グラフを描くコードを準備する

今回は、右斜め 45 度に伸びる直線のグラフを描きます。
:::message
コードに関する解説は行いません。別の記事で紹介します。
:::

```python
# matplotlibを利用できるようにインポートする
import matplotlib.pyplot as plt

# グラフとして描画するデータ
x = [1,2,3,4]
y = [2,3,4,5]

# グラフを描画
plt.plot(x, y)
plt.show()
```

### 2. グラフを表示する

1. VScode で作業中のファイルを開き、そのファイル上で**右クリック**を押してください。
2. 下記画像のように候補の中に**ターミナルで Python ファイルを実行する**をクリックする。
   ![ターミナルでPythonファイルを実行する](/images/articles/python-matplotlib-vscode/run-matplotlib-vscode.png)

### 3. 表示されたグラフを確認する。

期待通りのグラフが画像として表示されました。
![右斜め45度に伸びる直線のグラフ](/images/articles/python-matplotlib-vscode/45graph.png)

## 🌱 おわりに

『Python2 年生 データ分析のしくみ 体験してわかる、会話でまなべる』は、データ分析を初めて学習する方にとってわかりやすい書籍です。
興味がある方は購入をおすすめします。
興味がある方は、購入をおすすめします。
