---
title: '[TypeScript] 配列の各要素に対して処理できるmap関数' # 記事のタイトル
emoji: '🛡' # アイキャッチとして使われる絵文字（1文字だけ）
type: 'tech' # tech: 技術記事 / idea: アイデア記事
topics: ['typescript', '初心者向け'] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開設定（falseにすると下書き）
---

## 🌱 はじめに

この記事では、コードをシンプルに書ける**map 関数**を解説します。

:::details 参考資料
@[card](https://oukayuka.booth.pm/items/2368045)
:::

## 🌱 結論

:::message
配列の各要素に対して**指定した処理**を行い、その結果を**新しい配列として返すメソッド**

```ts
const hoge = list.map((各要素) => 指定した処理);
```

**メリット**

1. for ループなどを使用する場合よりも、**シンプルなコード**を書くことができます
2. 配列の各要素に対して**同じ処理**を実行する場合に非常に便利です
3. **元の配列を変更せず**に新しい配列を返すため、安全に配列を変更することができます

:::

## 🌱 サンプルコード

数字の配列の中身を 2 倍にするサンプルコードになります。

```typescript
// 数字の配列
const numbers = [1, 2, 3, 4, 5];
// 配列の値を2倍にする
const double = numbers.map((num) => num * 2);

console.log(double);
```

:::details 実行結果を確認する

```bash
[ 2, 4, 6, 8, 10 ]
```

:::
