---
title: '[Python] CSVファイルを読み込む2パターン' # 記事のタイトル
emoji: '🐍' # アイキャッチとして使われる絵文字（1文字だけ）
type: 'tech' # tech: 技術記事 / idea: アイデア記事
topics: ['python', 'csv', 'pandas'] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開設定（falseにすると下書き）
---

## はじめに

CSV ファイルを読み込む処理を一度書いたらその関数を使いまわすので、
忘れがちなので、備忘録として執筆します。

| 項目             | 内容                                   |
| ---------------- | -------------------------------------- |
| **対象者**       | ・Python 初学者                        |
| **伝えたい内容** | ・Python で CSV ファイルを読み込む方法 |
| **前提条件**     | ・Python 3.9.10<br>・pandas 1.5.2      |

### 対象の CSV ファイル

```csv: sample.csv
No,name,age,gender
1,ito,15,male
2,suzuki,14,female
3,sakai,18,male
```

| No  | name   | age | gender |
| --- | ------ | --- | ------ |
| 1   | ito    | 15  | male   |
| 2   | suzuki | 14  | female |
| 3   | sakai  | 18  | male   |

### ファイル構成

```bash
 .
 ├── csv
 │   └── sample.csv
 ├── csv_reader001.py
 ├── csv_reader002.py
 └── csv_reader003.py
```

### それぞれの違い

| ライブラリー        | インストール | コード量 | 返り値                     |
| ------------------- | ------------ | -------- | -------------------------- |
| 標準ライブラリー    | 不要         | 多め     | リスト型                   |
| pandas ライブラリー | 必要         | 少ない   | データフレームオブジェクト |

- **Q: データフレーム(DataFrame)オブジェクトとは？**
  - A: DataFrame は、行と列からなる 2 次元のデータ構造です。

## 標準ライブラリーで読み込む方法

### 1. 各行を 1 行ずつ出力するパターン

サンプルコード

```python: csv_reader001.py
# csvライブラリーのインポート
import csv

# csvファイルのファイルパスを引数に渡す
# 読み込んだファイル情報を`f`として扱う
with open('csv/sample.csv') as f:
    # readerでCSVファイルとして呼び出す
    reader = csv.reader(f)
    # reader: 2次元配列
    for row in reader:
        print(row)
```

出力結果を確認します

```bash
$python csv_reader001.py
['No', 'name', 'age', 'gender']
['1', 'ito', '15', 'male']
['2', 'suzuki', '14', 'female']
['3', 'sakai', '18', 'male']
```

### 2. 各行を 1 行ずつを配列の要素として出力するパターン

サンプルコード

```python: csv_reader002.py
from pprint import pprint
# csvライブラリーのインポート
import csv

# csvファイルのファイルパスを引数に渡す
# 読み込んだファイル情報を`f`として扱う
with open('csv/sample.csv') as f:
    # readerでCSVファイルとして呼び出す
    reader = csv.reader(f)
    reader_list = [row for row in reader]
    pprint(reader_list)
```

出力結果を確認します

```bash
$python csv_reader002.py
[['No', 'name', 'age', 'gender'],
 ['1', 'ito', '15', 'male'],
 ['2', 'suzuki', '14', 'female'],
 ['3', 'sakai', '18', 'male']]
```

## pandas ライブラリーで読み込む方法

- **Q: Python の pandas とは？**
  - A: Python のデータ解析ライブラリの一つであり、**表形式のデータを効率的**に扱う事が出来ます。
- **Q: pandas のメリットは？**
  - A: 行と列からなる表形式のデータの**読み込み、加工、分析、可視化**などを簡単に行う事が可能です。

pandas ライブラリーをインストールする

```bash
pip install pandas
```

サンプルコード

```python: csv_reader003.py
# pandasライブラリーのインポート
import pandas as pd

# csvファイルのファイルパスを引数に渡す
df = pd.read_csv('csv/sample.csv')
# 出力する
print(df)
```

出力結果を確認します

```bash
$python csv_reader002.py
     name  age  gender
0     ito   15    male
1  suzuki   14  female
2   sakai   18    male
```

## YouTube のご案内

ポモドーロタイマー（25 分勉強＋ 5 分休憩）を活用した作業・勉強配信を行っています。
集中したいときや、誰かと一緒に頑張りたいときに、ぜひご活用ください。

ご興味のある方は、ぜひお気軽に遊びに来てください！

@[card](https://www.youtube.com/@aew2sbee)
