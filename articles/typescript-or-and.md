---
title: "[TypeScript] これ('||')とそれ('&&')って何？" # 記事のタイトル
emoji: '🔰' # アイキャッチとして使われる絵文字（1文字だけ）
type: 'tech' # tech: 技術記事 / idea: アイデア記事
topics: ['typescript', '初心者向け'] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開設定（falseにすると下書き）
---

## はじめに

この記事では、TypeScriptの`||`,`&&`の**ショートサーキット評価**を解説します。

:::details 参考資料
@[card](https://oukayuka.booth.pm/items/2368045)
:::


## 結論

:::message
`||`は**OR 演算子**、`&&`は**AND 演算子**という意味になります。

**OR 演算子**
- 左項が**falsy な値**の場合、**右項の値**が使用される
- **※falsy な値**: `false`, `0n`, `0`, `-0`, `undefined`, `null`, `NaN`, `""`

**AND 演算子**
- 左項が**truthy な値**の場合、**右項の値**が使用される
- ※途中で**falsy な値**の場合、その値で決定される

:::

## 1. OR 演算子(`||`)

1. 初項の`undefined`は**falsy な値**のため、次項の`null`が対象となる
2. `null`は**falsy な値**のため、次項の`0`が対象となる
3. `0`は**falsy な値**のため、次項の`NaN`が対象となる
4. `NaN`は**falsy な値**のため、次項の`foo`が**最終値**として決定する

```typescript
const foo = undefined || null || 0 || NaN || '' || 'foo';
console.log(foo);
```

:::details 実行結果を確認する
```bash
"foo"
```
:::

## 2. AND 演算子(`&&`)

1. 初項の`100`は**truthy な値**のため、次項の`[]`が対象となる
2. `[]`は**truthy な値**のため、次項の`{}`が対象となる
3. `{}`は**truthy な値**のため、次項の`hoge`が**最終値**として決定する

```typescript
const hoge = 100 && [] && {} && 'hoge';
console.log(hoge);
```

:::details 実行結果を確認する
```bash
"hoge"
```
:::
