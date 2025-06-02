---
title: '[TypeScript] 配列の条件に一致する最初の要素を取得するfind関数' # 記事のタイトル
emoji: '🛡' # アイキャッチとして使われる絵文字（1文字だけ）
type: 'tech' # tech: 技術記事 / idea: アイデア記事
topics: ['typescript', '初心者向け'] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開設定（falseにすると下書き）
---

## はじめに

この記事では、**TypeScript の find** をまとめております。
:::details 参考資料
@[card](https://oukayuka.booth.pm/items/2368045)
:::

## 結論

:::message
配列の各要素に対して**指定された条件に一致する最初の要素**を返すメソッド

※要素が存在しない場合は`undefined`を返します。
しかし、文末に**?? 任意の値**を記述すると任意の値を返す事が出来る

```ts
const hoge = list.find((各要素) => 条件式);
```

**メリット**

1. for ループなどを使用する場合よりも、**シンプルなコード**を書くことができます
2. 要素が見つからなかった場合は undefined を返すため、**エラー処理**が簡単に行えます。
3. 配列内の要素が多数ある場合でも、検索が*高速*に行えます。
   :::

## 1. 条件を満たす

```ts
// 数字の配列
const numbers = [1, 2, 3, 4, 5];
// 2より大きい値を取得する(最初にヒットした値を取得する)
const result = numbers.find((i) => i > 2);

// 期待値： 3
console.log(result);
```

:::details 実行結果を確認する

```bash
3
```

:::

## 2. 条件を満たさない

```typescript
// 数字の配列
const numbers = [1, 2, 3, 4, 5];
// 10より大きい値を取得する
const result = numbers.find((i) => i > 10);

// 期待値： undefined
console.log(result);
```

:::details 実行結果を確認する

```bash
undefined
```

:::

## 3. 条件を満たさないかつ undefined を返さない

```typescript
// 数字の配列
const numbers = [1, 2, 3, 4, 5];
// 10より大きい値を取得する
const result = numbers.find((i) => i > 10) ?? false;

// 期待値： false
console.log(result);
```

:::details 実行結果を確認する

```bash
false
```

:::

## YouTube のご案内

ポモドーロタイマー（25 分勉強＋ 5 分休憩）を活用した作業・勉強配信を行っています。
集中したいときや、誰かと一緒に頑張りたいときに、ぜひご活用ください。

ご興味のある方は、ぜひお気軽に遊びに来てください！
「Zenn から来ました!!」とコメントを貰えると泣いて喜びます 🤣

@[card](https://www.youtube.com/@aew2sbee)
