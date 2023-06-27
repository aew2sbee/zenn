---
title: "【TypeScript】オブジェクト(object)を配列に変換したい" # 記事のタイトル
emoji: "🤹" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア記事
topics: ["typescript", "object"] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開設定（falseにすると下書き）
---
## はじめに
オブジェクトを配列にする方法(**反復処理**)を学びましたので、執筆しました。

|  項目  | 内容  |
| ---- | ---- |
|  **対象者**  |  ・オブジェクトを配列に変換したい方  |
|  **この記事でわかること**  |  ・オブジェクトの反復処理の使い方が分かる  |
|  **前提条件**  |  ・TypeScript 4.8.4 |

### 結論
|  種類  |  処理説明  |  メリット  |
| ---- | ---- | ---- |
|  Object.keys()  |  オブジェクトの**key**のみを取得する  |  1行で処理する事が出来る(for文不要)<br>map()などの処理にも併せて利用する事が出来る  |
|  Object.values()  |  オブジェクトの**value**のみを取得する  |  同上  |
|  Object.entries()  |  オブジェクトの**key**と**value**のみを取得する  |  同上  |

## Object.keysのサンプルコード
user_infoの**key**のみを取得します。
```typescript
const user_info = {
    name: "ogura",
    age: 18,
    gender: "female"
}
console.log(Object.keys(user_info));
```
出力結果を確認します。
```bash
["ogura", 18, "female"] 
```

## Object.valuesのサンプルコード
user_infoの**value**のみを取得します。
```typescript
const user_info = {
    name: "ogura",
    age: 18,
    gender: "female"
}

console.log(Object.values(user_info));
```
出力結果を確認します。
```bash
["ogura", 18, "female"] 
```
## Object.entriesのサンプルコード
user_infoの**key**と**value**のみを取得します。
```typescript
const user_info = {
    name: "ogura",
    age: 18,
    gender: "female"
}

console.log(Object.entries(user_info));
```
出力結果を確認します。
```bash
[["name", "ogura"], ["age", 18], ["gender", "female"]] 
```

## おわりに
オブジェクトの反復処理のイメージで反復練習が必要な🤹を連想しました。

