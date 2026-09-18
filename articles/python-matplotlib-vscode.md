---
title: "[Python] VS Codeでmatplotlibのグラフを表示する方法" # 記事のタイトル
emoji: "🐍" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア記事
topics: ["python", "matplotlib", "vscode", "データ分析"] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開:true / 非公開:false
---

## 🌱 はじめに

Python のデータ分析の学習を始めたいと思い、
『Python2 年生 データ分析のしくみ 体験してわかる、会話でまなべる』を購入しました。

本書は **Jupyter Notebook** を使って学習を進める構成で、
VS Code 上での学習方法は記載されていませんでした。

さらに、VS Code からグラフを表示する方法がわからず苦戦しました。
**他の方も苦戦しているのではないか**と思い、記録として残します。

## 🌱 結論

:::message
Python ファイルを右クリックし、下記画像のように候補の中から**ターミナルで Python ファイルを実行する**をクリックします。
事前に VS Code の Python 拡張機能と matplotlib のインストールが必要です。

![ターミナルでPythonファイルを実行する](/images/articles/python-matplotlib-vscode/run-matplotlib-vscode.png)
:::

### 前提条件

- Python 3.9.10
- Visual Studio Code（以下、VS Code）
- VS Code の Python 拡張機能（Microsoft 製）
- matplotlib 3.6.2
- グラフのウィンドウを表示できるローカルのデスクトップ環境

:::message
WSL や Remote-SSH などのリモート環境では、グラフのウィンドウが表示されない場合があります。
:::

## 🌱 解説

### 0. matplotlib をインストールする

VS Code のメニューから「ターミナル > 新しいターミナル」を開き、下記コマンドを実行します。

```bash
pip install matplotlib==3.6.2
```

### 1. グラフを描くコードを準備する

今回は、右上がりの直線（傾き 1）のグラフを描きます。
任意のフォルダに `graph.py` などの `.py` ファイルを作成し、下記コードを貼り付けて保存してください。

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

1. VS Code で作成したファイルを開き、エディタ上で**右クリック**してください。
2. 下記画像のように、候補の中から**ターミナルで Python ファイルを実行する**をクリックしてください。
   ![ターミナルでPythonファイルを実行する](/images/articles/python-matplotlib-vscode/run-matplotlib-vscode.png)

### 3. 表示されたグラフを確認する

期待通りのグラフが別ウィンドウに表示されました。
ウィンドウを閉じると、プログラムが終了します。

![右上がりの直線のグラフ](/images/articles/python-matplotlib-vscode/45graph.png)

## 🌱 おわりに

『Python2 年生 データ分析のしくみ 体験してわかる、会話でまなべる』は、データ分析を初めて学習する方にとってわかりやすい書籍です。
興味がある方は、購入をおすすめします。
