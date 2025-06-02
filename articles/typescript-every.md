---
title: '[TypeScript] 配列の全ての要素を判定するevery関数' # 記事のタイトル
emoji: '🛡' # アイキャッチとして使われる絵文字（1文字だけ）
type: 'tech' # tech: 技術記事 / idea: アイデア記事
topics: ['typescript', '初心者向け'] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開設定（falseにすると下書き）
---

## はじめに

この記事では、**TypeScript の every** をまとめております。

:::details 参考資料
@[card](https://oukayuka.booth.pm/items/2368045)
:::

## 結論

:::message
配列の各要素を対して**全ての要素が条件を満たすか**を判定するメソッド

```ts
const hoge = list.every((各要素) => 条件式);
```

**メリット**

1. for ループなどを使用する場合よりも、**シンプルなコード**を書くことができます
2. 最初の条件を満たさない要素が見つかった時点で**false**を返すため、効率的に動作します。

:::

## 1. 条件を満たす

```ts
// 数字の配列
const numbers = [1, 2, 3, 4, 5];
// 配列の要素が全て0より大きい値であるかを確認する
const result = numbers.every((i) => i > 0);

// 期待値： true
console.log(result);
```

:::details 実行結果を確認する

```bash
true
```

:::

## 2. 条件を満たさない

```typescript
// 数字の配列
const numbers = [1, 2, 3, 4, 5];
// 配列の要素が全て1より大きい値であるかを確認する
const result = numbers.every((i) => i > 1);

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
