---
title: "[テスト] pictの使い方" # 記事のタイトル
emoji: '🧪' # アイキャッチとして使われる絵文字（1文字だけ）
type: 'tech' # tech: 技術記事 / idea: アイデア記事
topics: ['テスト', 'pict', '初心者向け'] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開設定（falseにすると下書き）
---

## はじめに
この記事では、**Windows10上でpictの使い方** をまとめております。
:::details 参考資料
@[card](https://gihyo.jp/magazine/SD/archive/2024/202402)
:::

## 0. 事前準備
下記テストパターンでご紹介します
```txt:test.txt
Parameter1: Value1, Value2, Value3
Parameter2: ValueA, ValueB
Parameter3: Option1, Option2, Option3, Option4
```


## 1. [基本] Pairwise法で出力する
下記コマンドを実行する
```bash
pict test.txt
```
:::details 実行結果を確認する
```bash
$ pict test.txt
Parameter1      Parameter2      Parameter3
Value3  ValueA  Option1
Value1  ValueA  Option4
Value1  ValueB  Option3
Value2  ValueB  Option4
Value2  ValueA  Option3
Value2  ValueB  Option1
Value1  ValueB  Option1
Value3  ValueB  Option2
Value2  ValueA  Option2
Value3  ValueA  Option4
Value3  ValueB  Option3
Value1  ValueA  Option2
```
:::

## 2. [基本] テストパターンの総数を算出する
下記コマンドを実行する
```bash
pict test.txt -s
```
:::details 実行結果を確認する
```bash
$ pict test.txt -s
Combinations:   26
Generated tests:12
Generation time:0:00:00
```
:::

## 3. [応用] テストパターンに条件を追加する
### 1. IF条件: 条件Aが○○の場合、必ず条件Ｂを××にする
```md
Parameter1: Value1, Value2, Value3
Parameter2: ValueA, ValueB
Parameter3: Option1, Option2, Option3, Option4

IF [Parameter1] = "Value1" THEN [Parameter2] = "ValueA";
```

:::details 実行結果を確認する
```bash
$ pict test.txt
Parameter1      Parameter2      Parameter3
Value3  ValueA  Option1
Value1  ValueA  Option4
Value3  ValueB  Option3
Value3  ValueB  Option4
Value1  ValueA  Option3
Value2  ValueB  Option1
Value2  ValueA  Option3
Value2  ValueA  Option2
Value1  ValueA  Option1
Value2  ValueA  Option4
Value3  ValueB  Option2
Value1  ValueA  Option2
```
:::
:::details 条件追加による差分
```diff:bash
$ pict test.txt -s
- Combinations:   26
+ Combinations:   25
Generated tests:12
Generation time:0:00:00
```
:::


### 2. IF-ELSE条件: 条件Aが○○の場合、必ず条件Ｂを××でそれ以外の条件は、△△にする

```md
Parameter1: Value1, Value2, Value3
Parameter2: ValueA, ValueB
Parameter3: Option1, Option2, Option3, Option4

IF [Parameter1] = "Value1" THEN [Parameter2] = "ValueA" ELSE [Parameter2] = "ValueB";
```

:::details 実行結果を確認する
```bash
$ pict test.txt
Parameter1      Parameter2      Parameter3
Value3  ValueB  Option1
Value1  ValueA  Option4
Value2  ValueB  Option4
Value3  ValueB  Option3
Value3  ValueB  Option4
Value1  ValueA  Option1
Value2  ValueB  Option3
Value2  ValueB  Option2
Value1  ValueA  Option2
Value1  ValueA  Option3
Value2  ValueB  Option1
Value3  ValueB  Option2
```
:::
:::details 条件追加による差分
```diff:bash
$ pict test.txt -s
- Combinations:   26
+ Combinations:   23
Generated tests:12
Generation time:0:00:00
```
:::

### 3. IF-NOT条件: 条件Aが○○の場合、必ず条件Ｂを××にしない

```md
Parameter1: Value1, Value2, Value3
Parameter2: ValueA, ValueB
Parameter3: Option1, Option2, Option3, Option4

IF [Parameter1] = "Value1" THEN NOT [Parameter2] = "ValueA";
```


:::details 実行結果を確認する
```bash
$ pict test.txt
Parameter1      Parameter2      Parameter3
Value3  ValueA  Option1
Value1  ValueB  Option4
Value3  ValueB  Option3
Value3  ValueA  Option4
Value1  ValueB  Option3
Value2  ValueB  Option1
Value2  ValueA  Option3
Value2  ValueA  Option2
Value1  ValueB  Option1
Value2  ValueA  Option4
Value3  ValueB  Option2
Value1  ValueB  Option2
```
:::
:::details 条件追加による差分
```diff:bash
$ pict test.txt -s
- Combinations:   26
+ Combinations:   25
Generated tests:12
Generation time:0:00:00
```
:::
