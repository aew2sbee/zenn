---
title: "【Python】pprintの使い方" # 記事のタイトル
emoji: "🐍" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア記事
topics: ["python", "pprint"] # タグ。["markdown", "rust", "aws"]のように指定する
published: false # 公開設定（falseにすると下書き）
---
## はじめに
**ネストの深いJson/辞書型のデータ**

文字出力のprint関数より便利なpprint関数を知ったので共有します。

### 前提条件

### 環境
- Python 3.9.10

### 対象者
- ネストが深いJson/辞書型のデータを見やすく出力する方法を知りたい方
- Pythonのpprintの使い方を知りたい方

### 出来るようになること
- pprintの使い方が理解できる


## 手順
### 1.pprintをインポートする
```python
from pprint import pprint
```
2. 出力するデータを準備する
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
3. データを出力する
```python
pprint(data)
```

## サンプルコード
```python
from pprint import pprint

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

pprint(data)
```

```bash
----- 出力結果 -----
{'student1': {'age': 15, 'gender': 'male', 'name': 'ito'},
 'student2': {'age': 14, 'gender': 'female', 'name': 'suzuki'},
 'student3': {'age': 18, 'gender': 'male', 'name': 'sakai'}}
```
## おわりに