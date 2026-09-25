---
title: "[TypeScript] 配列の条件を満たす要素で新しい配列を作成するfilter関数" # 記事のタイトル
emoji: "🛡" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア記事
topics: ["typescript", "初心者向け"] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開:true / 非公開:false
---

## 🌱 はじめに

この記事では、**TypeScript の filter** をまとめています。
`filter`は JavaScript の配列メソッド（`Array.prototype.filter`）で、TypeScript でもそのまま使えます

@[card](https://oukayuka.booth.pm/items/2368045)

## 🌱 結論

:::message
配列から**条件に一致する要素だけを取り出した新しい配列を作成するメソッド**

```ts
const hoge = list.filter((各要素) => 条件式);
```

**メリット**

1. for ループなどを使用する場合よりも、**シンプルなコード**を書くことができます。
2. 引数に渡す関数の戻り値が true（truthy）になる要素を抽出するため、**フィルタリング条件を自由に設定できます**
3. **元の配列を変更せず**に新しい配列を返します（ただし、要素がオブジェクトの場合は、元の配列と同じオブジェクトを参照します）
:::

## 🌱 1. 条件を満たす

数字の配列から**偶数**のみを取得する

```ts
// 数字の配列
const numbers = [1, 2, 3, 4, 5];
// 偶数の値を抽出して新しい配列とする
const even = numbers.filter((i) => i % 2 === 0);

console.log(even);
```

:::details 実行結果を確認する

```text
[ 2, 4 ]
```

:::

## 🌱 2. 条件を満たす要素がない

数字の配列から 10 で割り切れる数値のみを取得する（該当する要素がない場合は、空の配列が返る）

```ts
// 数字の配列
const numbers = [1, 2, 3, 4, 5];
// 10で割り切れる値を抽出して新しい配列とする
const res = numbers.filter((i) => i % 10 === 0);

console.log(res);
```

:::details 実行結果を確認する

```text
[]
```

:::
