---
title: '[Python] 見づらいJsonデータを解決する方法' # 記事のタイトル
emoji: '🐍' # アイキャッチとして使われる絵文字（1文字だけ）
type: 'tech' # tech: 技術記事 / idea: アイデア記事
topics: ['python', 'pprint', 'Json'] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開設定（falseにすると下書き）
---

## はじめに

Python で開発している時に
print 関数で下記のような**見づらい Json データ**を出力する時ありませんか？

```git
{'student1': {'name': 'ito', 'age': 15, 'gender': 'male'}, 'student2': {'name': 'suzuki', 'age': 14, 'gender': 'female'}, 'student3': {'name': 'sakai', 'age': 18, 'gender': 'male'}}
```

今回は、この見づらい Json データを**見やすいよう**に出力する方法をご紹介します。

### 対象読者

- 見づらい Json データを改善したい方
- pprint の使い方を知りたい方

### この記事でわかること

- 見づらい Json データを見やすいく出力する方法
- pprint の使い方を理解できる

### 前提条件

- Python 3.9.10

## 手順解説

### 1. インポート文を記載する

使用するファイルの上段に下記を記載する

```python
from pprint import pprint
```

### 2. 出力するデータを準備する

出力用に下記データを用意しました。

```python
data = {
    "student1": {
        "name": "ito",
        "age": 15,
        "gender": "male"
    },
    "student2": {
        "name": "suzuki",
        "age": 14,
        "gender": "female"
    },
    "student3": {
        "name": "sakai",
        "age": 18,
        "gender": "male"
    }
}
```

### 3. データを出力する

**pprint()** で先ほどのデータを出力します。

```python
pprint(data)
```

## サンプルコード

先ほどの手順解説をまとめたコードになります。

```python:pprint.py
# import pprintでも可能ですが、利用するときは、pprint.pprint()とする必要があります。
from pprint import pprint

# 出力用データ
data = {
    "student1": {
        "name": "ito",
        "age": 15,
        "gender": "male"
    },
    "student2": {
        "name": "suzuki",
        "age": 14,
        "gender": "female"
    },
    "student3": {
        "name": "sakai",
        "age": 18,
        "gender": "male"
    }
}

# ターミナルに出力する
pprint(data)
```

```bash
$ python pprint.py
{'student1': {'age': 15, 'gender': 'male', 'name': 'ito'},
 'student2': {'age': 14, 'gender': 'female', 'name': 'suzuki'},
 'student3': {'age': 18, 'gender': 'male', 'name': 'sakai'}}
```

## おわりに

pprint を知らないときは、ターミナルのデータをコピーして
メモ帳で手動で改行しておりました。
pprint を知ってから開発スピードが上がりました！

ちなみに、
pprint とは、**pretty-print**（きれいに表示する）の略語で、
**データ構造を見やすく整形して表示するためのライブラリ** です。

## YouTube のご案内

ポモドーロタイマー（25 分勉強＋ 5 分休憩）を活用した作業・勉強配信を行っています。
集中したいときや、誰かと一緒に頑張りたいときに、ぜひご活用ください。

ご興味のある方は、ぜひお気軽に遊びに来てください！
「Zenn から来ました!!」とコメントを貰えると泣いて喜びます 🤣

@[card](https://www.youtube.com/@aew2sbee)
