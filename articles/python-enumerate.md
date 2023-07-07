---
title: "【Python】便利な関数(enumerate)でforループのindexをカウントしてくれる" # 記事のタイトル
emoji: "📏" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア記事
topics: ["python", "enumerate"] # タグ。["markdown", "rust", "aws"]のように指定する
published: false # 公開設定（falseにすると下書き）
---
## はじめに
配列の各要素を対して全ての要素が条件を満たすかを判定する**every関数**を学びましたので、執筆しました。

|  項目  | 内容  |
| ---- | ---- |
|  **対象者**  |  ・TypeScript初学者  |
|  **伝えたい内容**  |  ・everyの使い方が分かる  |
|  **前提条件**  |  ・Python 3.10.11 |

### 結論
:::message
イテラブル（リスト、タプル、文字列など）の要素にインデックスを付けて取得するための関数です
```python
for index, value in enumerate(配列):
```
:::

### メリット
1. forループなどを使用する場合よりも、**シンプルなコード**を書くことができます
2. 短くて簡潔なコード: 値とインデックスを同時に処理するために、複数の変数や一時変数を作成する必要がありません。
3. 読みやすいコード: enumerate()関数を使用することで、ループ内で要素のインデックスが明示的に表示されるため、コードの意図がより明確になります。他の人がコードを読んだり、自分が後でコードを見直す際にも役立ちます。

## 1. every()で条件を満たすサンプルコード
```python
fruits = ['apple', 'banana', 'orange']
    print(index, fruit)
```
出力結果を確認します。
```bash
true
```

## 2. every()で条件を満たさないサンプルコード
```python
// 数字の配列
const numbers = [1,2,3,4,5];
// 配列の要素が全て1より大きい値であるかを確認する
const result = numbers.every((i) => i > 1)

// 期待値： false
console.log(result)
```
出力結果を確認します。
```bash
false
```

## おわりに
.every()といえば、news every.をイメージして📏のemojiにしました。

