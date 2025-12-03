---
title: "[Python] 3種類の内包表記" # 記事のタイトル
emoji: "🐍" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア記事
topics: ["python", "内包表記", "初心者向け"] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開設定（falseにすると下書き）
---

## はじめに

この記事では、`Python`の**内包表記**についてをまとめております。

:::details 参考資料
@[card](https://gihyo.jp/magazine/SD/archive/2025/202502)
:::

## 結論

:::message

- **リスト内包式**: シンプルなシーケンス
- **集合内包式**: 重複を排除したい
- **辞書内包表記**: キーと値をまとめたい

```py
# リスト内包式
[ 式 for 変数リスト in イテラブル if 条件式 ]

# 集合内包式
{ 式 for 変数リスト in イテラブル if 条件式  }

# 辞書内包
{ key: value for 変数リスト in イテラブル if 条件式  }

```

:::

## 1. リスト内包式

```py
# 0〜9 の偶数の二乗リストを作る
squares = [x*x for x in range(10) if x % 2 == 0]
```

:::details 実行結果を確認する

```bash
[0, 4, 16, 36, 64]
```

:::

## 2. 集合内包式

```py
# 文字列中の英字を大文字にしてユニークな集合を作る
chars = {c.upper() for c in "abracadabra" if c.isalpha()}
```

:::details 実行結果を確認する

```bash
{'A', 'B', 'C', 'D', 'R'}
```

:::

## 3. 辞書内包

```py
# 0〜4 の数字をキーとし、その二乗を値に持つ辞書を作る
square_dict = {x: x*x for x in range(5)}
```

:::details 実行結果を確認する

```bash
{0: 0, 1: 1, 2: 4, 3: 9, 4: 16}
```

:::
