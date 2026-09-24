---
title: "[テスト] pictの使い方" # 記事のタイトル
emoji: "🧪" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア記事
topics: ["テスト", "pict", "初心者向け"] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開:true / 非公開:false
---

## 🌱 はじめに

この記事では、**Windows10 上での PICT の使い方** をまとめています。
:::details 参考資料
@[card](https://gihyo.jp/magazine/SD/archive/2024/202402)
:::

## 🌱 0. 事前準備

PICT の導入方法は、下記記事を参照してください。コマンドは、すべて Git Bash で実行します。

@[card](https://zenn.dev/aew2sbee/articles/test-pict-install)

任意の作業フォルダに、下記内容で`test.txt`（PICT に読み込ませるモデルファイル）を作成します。

```txt:test.txt
Parameter1: Value1, Value2, Value3
Parameter2: ValueA, ValueB
Parameter3: Option1, Option2, Option3, Option4
```

## 🌱 1. [基本] Pairwise 法で出力する

下記コマンドを実行する

```bash
pict test.txt
```

:::details 実行結果を確認する

※ PICT Version 3.7.4 での出力例です。行の順番はバージョンによって変わる場合があります。

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

## 🌱 2. [基本] モデルの統計情報（ペア数と生成したテスト数）を表示する

下記コマンドを実行する（オプションは`/s`と`-s`のどちらの形式でも指定できます）

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

- `Combinations`: 網羅すべき 2 つのパラメーターの値のペアの数（Parameter1×Parameter2 の 6 通り＋Parameter1×Parameter3 の 12 通り＋Parameter2×Parameter3 の 8 通り＝26）
- `Generated tests`: 生成されたテストケースの数

## 🌱 3. [応用] テストパターンに条件を追加する

以降の各節では、`test.txt`を下記の内容に書き換えて（前の節で追加した条件は削除して）、`pict test.txt`と`pict test.txt -s`を実行します。

### 1. IF 条件: 条件 A が ○○ の場合、必ず条件 B を ×× にする

```txt:test.txt
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

```diff bash
$ pict test.txt -s
- Combinations:   26
+ Combinations:   25
Generated tests:12
Generation time:0:00:00
```

:::

### 2. IF-ELSE 条件: 条件 A が ○○ の場合は必ず条件 B を ×× にし、それ以外の場合は △△ にする

```txt:test.txt
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

```diff bash
$ pict test.txt -s
- Combinations:   26
+ Combinations:   23
Generated tests:12
Generation time:0:00:00
```

:::

### 3. IF-NOT 条件: 条件 A が ○○ の場合、必ず条件 B を ×× にしない

```txt:test.txt
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

```diff bash
$ pict test.txt -s
- Combinations:   26
+ Combinations:   25
Generated tests:12
Generation time:0:00:00
```

:::
