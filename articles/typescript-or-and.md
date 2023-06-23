---
title: "【TypeScript】これ('||')とそれ('&&')って何？" # 記事のタイトル
emoji: "🔰" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア記事
topics: ["typescript"] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開設定（falseにすると下書き）
---
## はじめに
typescriptの学習を始めた時に、サンプルコードを見て`||`,`&&`って何これ？と思いました。
色々調べて`||`,`&&`を理解したので、執筆しします。

### 対象読者
- TypeScript初学者

### この記事でわかること
- TypeScriptのショートサーキット評価が分かる


### 前提条件
- TypeScrip 4.8.4

### 結論
:::message
`||`は**OR演算子**、`&&`は**AND演算子**という意味になります。
:::

## サンプルコード
### OR演算子(`||`)
:::message
左項が**falsyな値**の場合、**右項の値**が使用される
**※falsyな値**: `false`, `0n`, `0`, `-0`, `undefined`, `null`, `NaN`, `""`
:::
####  fooに代入される値の解説
1. 初項の`undefined`は**falsyな値**のため、次項の`null`が対象となる
2. `null`は**falsyな値**のため、次項の`0`が対象となる
3. `0`は**falsyな値**のため、次項の`NaN`が対象となる
4. `NaN`は**falsyな値**のため、次項の`foo`が**最終値**として決定する
```typescript
const foo = undefined || null || 0 || NaN || "" || "foo";
console.log(foo);
```
```bash
----- 出力結果 -----
"foo" 
```

### AND演算子(`&&`)
:::message
左項が**truthyな値**の場合、**右項の値**が使用される
※途中で**falsyな値**の場合、その値で決定される
:::

####  hogeに代入される値の解説
1. 初項の`100`は**truthyな値**のため、次項の`[]`が対象となる
2. `[]`は**truthyな値**のため、次項の`{}`が対象となる
3. `{}`は**truthyな値**のため、次項の`hoge`が**最終値**として決定する
```typescript
const hoge = 100 && [] && {} && "hoge";
console.log(hoge);
```
```bash
----- 出力結果 -----
"hoge" 
```

## おわりに
`||`,`&&`ついて分からなくなった時は、この記事を読み返して理解します。