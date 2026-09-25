---
title: "[TypeScript] 配列の条件に一致する最初の要素を取得するfind関数" # 記事のタイトル
emoji: "🛡" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア記事
topics: ["typescript", "初心者向け"] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開:true / 非公開:false
---

## 🌱 はじめに

この記事では、**TypeScript の find** をまとめています。
`find`は JavaScript の配列メソッド（`Array.prototype.find`）で、TypeScript でもそのまま使えます。

@[card](https://oukayuka.booth.pm/items/2368045)

## 🌱 結論

:::message
配列から**指定された条件に一致する最初の要素**を返すメソッド

※条件に一致する要素が存在しない場合は`undefined`を返します。
ただし、`find(...)`の後ろに**?? 任意の値**を書くと、`undefined`の代わりに任意の値を返すことができます。

```ts
const hoge = list.find((各要素) => 条件式);
```

**メリット**

1. for ループなどを使用する場合よりも、**シンプルなコード**を書くことができます。
2. 要素が見つからなかった場合は`undefined`を返すため、**見つからなかった場合の処理**を書きやすくなります。
3. 条件に一致する要素が見つかった時点で検索を終えるため、残りの要素は判定しません。
:::

## 🌱 1. 条件を満たす

```ts
// 数字の配列
const numbers = [1, 2, 3, 4, 5];
// 2より大きい値を取得する(最初にヒットした値を取得する)
const result = numbers.find((i) => i > 2);

// 期待値： 3
console.log(result);
```

:::details 実行結果を確認する

```text
3
```

:::

## 🌱 2. 条件を満たさない

```ts
// 数字の配列
const numbers = [1, 2, 3, 4, 5];
// 10より大きい値を取得する
const result = numbers.find((i) => i > 10);

// 期待値： undefined
console.log(result);
```

:::details 実行結果を確認する

```text
undefined
```

:::

## 🌱 3. 条件を満たさない場合に undefined 以外の値を返す

```ts
// 数字の配列
const numbers = [1, 2, 3, 4, 5];
// 10より大きい値を取得する
const result = numbers.find((i) => i > 10) ?? false;

// 期待値： false
console.log(result);
```

※この場合、`result`の型は`number | false`になります。
※配列に`undefined`や`null`が含まれる場合は、「見つからなかった」のか「その要素が見つかった」のかを区別できません。その場合は`findIndex`や`some`を使います。

:::details 実行結果を確認する

```text
false
```

:::
