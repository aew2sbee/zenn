---
title: "【TypeScript】これ('=>')って何？" # 記事のタイトル
emoji: "➡" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア記事
topics: ["typescript"] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開設定（falseにすると下書き）
---
## はじめに
typescriptの学習を始めた時に、サンプルコードを見て`=>`って何これ？と思いました。
色々調べて便利である事が分かりまとめました。

|  項目  | 内容  |
| ---- | ---- |
|  **対象者**  |  ・`=>`について知りたい方  |
|  **伝えたい内容**  |  ・TypeScriptのアロー関数式が分かる  |
|  **前提条件**  |  ・TypeScript 3.8.3 |

### 結論
:::message
`=>`は、typescriptのアロー関数式という書き方になります。
functionを使用せず、**`=>`を使用した関数**のことです。
さらに、記法に**省略記法**があり、**シンプルかつ少ないコード**で記述することが出来ます。
:::

|    |  メリット  |
| ---- | ---- |
|  functionを使用した関数  |  ---  |
|  アロー関数式  |  **function**を省略する可能  |
|  アロー関数式 (省略記法)  |  **return**と **{}** を省略する可能  |

## サンプルコード
### functionを使用した関数
```typescript
// 機能内容: 渡された金額を消費税込みの金額に計算をする
const clac_tax = function (price: number){
  // Math.floor: 小数点を切り捨て
  return Math.floor(price * 1.1);
}
console.log(clac_tax(100))
```
```bash
----- 出力結果 -----
110
```

### アロー関数式
    
:::message
**function**を省略する事が出来ます
:::
```typescript
// 機能内容: 渡された金額を消費税込みの金額に計算をする
const clac_tax = (price: number) => {
  // Math.floor: 小数点を切り捨て
  return Math.floor(price * 1.1);
}
console.log(clac_tax(100))
```
```bash
----- 出力結果 -----
110
```

### アロー関数式 (省略記法)
:::message
**return**と **{}** を省略する事が出来、1行で記載する事が出来ます
:::

```typescript
// 機能内容: 渡された金額を消費税込みの金額に計算をする
// Math.floor: 小数点を切り捨て
const clac_tax = (price: number) => Math.floor(price * 1.1)
console.log(clac_tax(100))
```
```bash
----- 出力結果 -----
110
```



## おわりに
どの記述も同じ結果になりました。
アロー関数式 (省略記法)が少ないコード量でシンプルに表現できる為、この書き方をお勧めします。
現場によって記法は様々ですので、その現場にあった記法に合わせた方が良いでしょう。

