---
title: "【自動化テスト】sharepointでテスト設計書を管理するの辞めませんか？" # 記事のタイトル
emoji: "🧪" # アイキャッチとして使われる絵文字（1文字だけ）
type: "idea" # tech: 技術記事 / idea: アイデア記事
topics: ["python", "pytest", "csv", "tsv"] # タグ。["markdown", "rust", "aws"]のように指定する
published: false # 公開設定（falseにすると下書き）
---
## はじめに
約2年間Pythonで自動化テストを行うプロジェクトに参画しておりました。

大半のプロジェクトは、
**テスト設計書をExcelで作成し、sharepointに格納してみんなが確認できる環境**
で行っていると思います。
自分が参画していた上記のプロジェクトも同様の事を行っておりました。

### sharepointでテスト設計書を管理するデメリット
この環境について個人的に感じるデメリットを挙げさせて頂きます。
1. テストコードとテスト設計書が**別々の環境**で管理されている
    - Q:それの何がダメなの？
    - A:仕様変更等により、**片方だけが更新され、もう片方は古いまま**の可能性が上がる
2. テスト数が増えてくると、テストコードとテスト設計書の紐づけが難しくなってくる
    - Q:それの何がダメなの？
    - A:新規の方が

sharepointのファイルを**プレビュー/ダウンロード**をするルールを決めても、間違ってファイルを開き、更新されてしまう
今回は、テスト設計書をGit上で管理する


### 対象読者
- 
- Pythonの基本文法を理解している方
- pytestライブラリーの使い方を知っている方

### この記事でわかること
- 見づらいJsonデータを見やすいく出力する方法
- pprintの使い方を理解できる

### 前提条件
- Python 3.9.10
- ファイル構成

### ファイル構成
```bash
$ tree -a
pytest
├─ src
│   └─ calc.py  # 今回のテスト対象
└─ test
    ├─ csv
    │   └─ test_design.csv  # テスト試験書(CSV)
    ├─ func
    │   └─ test_common.py  # pytestに使う共通関数
    └─ test_four_arithmetic_operations.py  # テストコード
```

:::message alert
テストコードのファイル名は、留意する点があります!
ファイル名の先頭が **"test_"** で始めらないといけません。

下記のようなファイル名は、pytestの仕様上テストコードのファイルと認識しません。
- `testfour_arithmetic_operations.py`
- `atest_four_arithmetic_operations.py`
:::


## 手順解説
### 1. pytestライブラリーをインストールする

## サンプルコード
先ほどの手順解説をまとめたコードになります。
```python:calc.py
class Calc():

    def four_arithmetic_operations(x, y, action=None):
        # 加算
        if action == "add":
            return x + y
        # 減算
        elif action == "sub":
            return x - y
        # 掛け算
        elif action == "mul":
            return x * y
        # 割り算
        elif action == "div":
            return x / y
        # 例外
        else:
            raise ValueError("四則演算以外の操作です")
```

```bash
$ python pprint.py

```

:::message
なぜ、拡張子を`csv`ではなく`tsv`を選択しているのか？

:::

## おわりに
