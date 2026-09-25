---
title: "[Python] 3種類の内包表記" # 記事のタイトル
emoji: "🐍" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア記事
topics: ["python", "内包表記", "初心者向け"] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開:true / 非公開:false
---

## 🌱 はじめに

この記事では、`Python`の**内包表記**についてまとめています。

@[card](https://gihyo.jp/magazine/SD/archive/2025/202502)

## 🌱 結論

:::message

- **リスト内包表記**: 順序を保ち、重複を許すリストを作りたい
- **集合内包表記**: 重複を排除したい
- **辞書内包表記**: キーと値をまとめたい

```text
# リスト内包表記
[ 式 for 変数 in イテラブル if 条件式 ]

# 集合内包表記
{ 式 for 変数 in イテラブル if 条件式 }

# 辞書内包表記
{ キー: 値 for 変数 in イテラブル if 条件式 }
```

`if 条件式` は省略できます。
また、`x if 条件 else y` のような条件分岐は、末尾ではなく「式」の位置に書きます。

:::

:::message alert
丸括弧 `( )` で書くと、タプルではなくジェネレータ式になります。
:::

## 🌱 1. リスト内包表記

```py
# 0〜9 の偶数の二乗リストを作る
squares = [x*x for x in range(10) if x % 2 == 0]
print(squares)
```

:::details 実行結果を確認する

```text
[0, 4, 16, 36, 64]
```

:::

## 🌱 2. 集合内包表記

```py
# 文字列中の英字を大文字にしてユニークな集合を作る
chars = {c.upper() for c in "abracadabra" if c.isalpha()}
print(chars)
```

:::details 実行結果を確認する

```text
{'A', 'B', 'C', 'D', 'R'}
```

※集合は順序を持たないため、表示順は実行ごとに異なる場合があります。

:::

## 🌱 3. 辞書内包表記

```py
# 0〜4 の数字をキーとし、その二乗を値に持つ辞書を作る
square_dict = {x: x*x for x in range(5)}
print(square_dict)
```

:::details 実行結果を確認する

```text
{0: 0, 1: 1, 2: 4, 3: 9, 4: 16}
```

:::
