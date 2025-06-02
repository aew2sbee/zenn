---
title: '[TypeScript] 配列の条件を満たす要素で新しい配列を作成するfilter関数' # 記事のタイトル
emoji: '🛡' # アイキャッチとして使われる絵文字（1文字だけ）
type: 'tech' # tech: 技術記事 / idea: アイデア記事
topics: ['typescript', '初心者向け'] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開設定（falseにすると下書き）
---

## はじめに

この記事では、**TypeScript の filter** をまとめております。
:::details 参考資料
@[card](https://oukayuka.booth.pm/items/2368045)
:::

## 結論

:::message
配列の各要素に対して**条件に一致する要素だけを含む新しい配列を作成するメソッド**

```typescript
const hoge = list.filter((各要素) => 条件式);
```

**メリット**

1. for ループなどを使用する場合よりも、**シンプルなコード**を書くことができます
2. 引数に渡される関数の戻り値が true である要素を抽出するため、**フィルタリング条件を自由に設定できる**
3. **元の配列を変更せず**に新しい配列を返すため、安全に配列を変更することができます
   :::

## 1. 条件を満たす

数字の配列の中身で**偶数**のみを取得する

```ts
// 数字の配列
const numbers = [1, 2, 3, 4, 5];
// 偶数の値を抽出して新しい配列とする
const even = numbers.filter((i) => i % 2 === 0);

console.log(even);
```

:::details 実行結果を確認する

```bash
[ 2, 4 ]
```

:::

## 2. 条件を満たさない

数字の配列の中身で 10 で割れる数値のみを取得する

```ts
// 数字の配列
const numbers = [1, 2, 3, 4, 5];
// 偶数の値を抽出して新しい配列とする
const res = numbers.filter((i) => i % 10 === 0);

console.log(res);
```

:::details 実行結果を確認する

```bash
[]
```

:::

## YouTube のご案内

ポモドーロタイマー（25 分勉強＋ 5 分休憩）を活用した作業・勉強配信を行っています。
集中したいときや、誰かと一緒に頑張りたいときに、ぜひご活用ください。

ご興味のある方は、ぜひお気軽に遊びに来てください！
「Zenn から来ました!!」とコメントを貰えると泣いて喜びます 🤣

@[card](https://www.youtube.com/@aew2sbee)
