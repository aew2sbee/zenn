---
title: '[TypeScript] 便利な関数(filter)の使い方' # 記事のタイトル
emoji: '☕' # アイキャッチとして使われる絵文字（1文字だけ）
type: 'tech' # tech: 技術記事 / idea: アイデア記事
topics: ['typescript', 'filter'] # タグ。["markdown", "rust", "aws"]のように指定する
published: false # 公開設定（falseにすると下書き）
---

## はじめに

配列の各要素に対して条件の確認を行う**filter 関数**を学びましたので、執筆しました。

| 項目             | 内容                      |
| ---------------- | ------------------------- |
| **対象者**       | ・TypeScript 初学者       |
| **伝えたい内容** | ・filter の使い方が分かる |
| **前提条件**     | ・TypeScript 4.8.4        |

### 結論

:::message
配列の各要素に対して**条件に一致する要素だけを含む新しい配列を作成するメソッド**

```typescript
const hoge = list.filter((各要素) => 条件式);
```

:::

### メリット

1. for ループなどを使用する場合よりも、**シンプルなコード**を書くことができます
2. 引数に渡される関数の戻り値が true である要素を抽出するため、**フィルタリング条件を自由に設定できる**
3. **元の配列を変更せず**に新しい配列を返すため、安全に配列を変更することができます

## サンプルコード

数字の配列の中身で**偶数**のみを取得するサンプルコードになります。

```typescript
// 数字の配列
const numbers = [1, 2, 3, 4, 5];
// 偶数の値を抽出して新しい配列とする
const even = numbers.filter((i) => i % 2 === 0);

console.log(even);
```

出力結果を確認します。

```bash
[ 2, 4 ]
```

## おわりに

.filter()といえば、フィルターと通したコーヒー ☕ は美味しいですよね
