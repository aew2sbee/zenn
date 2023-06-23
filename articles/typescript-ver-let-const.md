---
title: "【TypeScript】ver/let/constの違い" # 記事のタイトル
emoji: "🆚" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア記事
topics: ["typescript"] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開設定（falseにすると下書き）
---
## はじめに
typescriptの学習を始めた時に、サンプルコードを見て`ver/let/const`の違いが分かりませんでした。
色々調べて`ver/let/const`の違いを理解したので、執筆しします。

### 対象読者
- TypeScript初学者

### この記事でわかること
- TypeScriptのver/let/constが分かる


### 前提条件
- TypeScript 3.8.3

### 結論
:::message
それぞれの違いは、**再代入**と**再宣言**が可能かどうかになります。

:::

|  変数宣言  |  意味  |  再代入  |  再宣言  |
| :---: | :--- | :---: | :---: |
|  var  |  varible(変数)  |  〇  |  〇  |
|  let  |  let it be (あるがままに)  |  〇  |  ×  |
|  const  |  constant (定数)  |  ×  |  ×  |


## 解説
### varible (変数)とは
下記の表の通りになります。

|  宣言  |  意味  |  再代入  |  再宣言  |
| :---: | :---: | :---: | :---: |
|  var  |  varible(変数)  |  〇  |  〇  |


#### 再代入
varの再代入についてサンプルコードとともに確認します。
```typescript
// 初回宣言
var price = 100;
console.log(price)

// 再代入
price = 120;
console.log(price)
```
varの再代入が可能である事が確認出来ました。
```bash
----- 出力結果 -----
100
120
```
#### 再宣言
varの再宣言についてサンプルコードとともに確認します。
```typescript
// 初回宣言
var price = 100;
console.log(price)

// 再宣言
var price = 200;
console.log(price)
```
varの再宣言が可能である事が確認出来ました。
```bash
----- 出力結果 -----
100
200
```

### let it be (あるがままに)とは
下記の表の通りになります。

|  宣言  |  意味  |  再代入  |  再宣言  |
| :---: | :---: | :---: | :---: |
|  let  |  let it be (あるがままに)  |  〇  |  ×  |


#### 再代入
letの再代入についてサンプルコードとともに確認します。
```typescript
// 初回宣言
let price = 100;
console.log(price)

// 再代入
price = 120;
console.log(price)
```
letの再代入が可能である事が確認出来ました。
```bash
----- 出力結果 -----
100
120
```
#### 再宣言
letの再宣言についてサンプルコードとともに確認します。
```typescript
// 初回宣言
let price = 100;
console.log(price)

// 再宣言
let price = 200;
console.log(price)
```
:::message alert
letを使って同じ変数を**再宣言**している
:::
```bash
----- 出力結果 -----
error TS2451: Cannot redeclare block-scoped variable 'price'.
```


### constant (定数)とは
下記の表の通りになります。

|  宣言  |  意味  |  再代入  |  再宣言  |
| :---: | :---: | :---: | :---: |
|  const  |  constant (定数)  |  ×  |  ×  |


#### 再代入
constの再代入についてサンプルコードとともに確認します。
```typescript
// 初回宣言
const price = 100;
console.log(price)

// 再代入
price = 120;
console.log(price)
```
:::message alert
constで定義している変数に対して**再代入**が発生している
:::
```bash
----- 出力結果 -----
error TS2588: Cannot assign to 'price' because it is a constant.
```
#### 再宣言
constの再宣言についてサンプルコードとともに確認します。
```typescript
// 初回宣言
const price = 100;
console.log(price)

// 再宣言
const price = 200;
console.log(price)
```
:::message alert
constを使って同じ変数を**再宣言**している
:::
```bash
----- 出力結果 -----
error TS2451: Cannot redeclare block-scoped variable 'price'.
```

## おわりに
基本的には潜在的なバグを生みにくい順番として**const > let > var**である
理由: 再代入/再宣言が可能なほど意図しない動作が発生するの為
→安定したプログラムを作成する時は、**const**を優先的に使用し、再代入したい場合は、**let**を使用する。

