---
title: '[TypeScript] エラー処理/例外処理の3つの方法' # 記事のタイトル
emoji: '🛡' # アイキャッチとして使われる絵文字（1文字だけ）
type: 'tech' # tech: 技術記事 / idea: アイデア記事
topics: ['typescript', '初心者向け'] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開設定（falseにすると下書き）
---

## はじめに

`TypeScript`をより深く理解したく下記書籍を読みました。
下記書籍の`Error処理/例外処理`について学びがあったので、記事として記録します。
@[card](https://www.oreilly.co.jp/books/9784873119045/)

## 結論

:::message

- 単純に Error を知らせる事を求めるなら = **null を返す**
- なぜ失敗したのかについてより多くの情報を求めるなら = **例外をスローする/例外を返す**
- 起こり得るすべての例外情報を強く望むなら = **例外を返す**
- 少ない記述で例外処理を求めるなら = **例外をスローする**

:::

## 1. null を返す

:::message
型安全な方法でエラーを処理するために**最も軽量な方法**
:::
:::message alert
複数の操作を組み立てが困難になる
エラーの有無しかわからない(発生理由までは追加で調査要)
**冗長**になりやすい
:::

```ts
const parse = (birthday: string): Date => {
  let data = new Date(birthday);
  if (isVaild(birthday)) {
    return null;
  }
  return data;
};
```

## 2. 例外をスローする

:::message
null を返すより、**より簡単にデバック**が可能
**特定の失敗モード**を処理が出来る
:::
:::message alert
標準的なエラーオブジェクトから選び必要がある(カスタマイズが難しい)
:::

```ts
const parse = (birthday: string): Date => {
  let data = new Date(birthday);
  if (isVaild(birthday)) {
    throw new RangeError('Enter a data in the form YYYY/MM/DD');
  }
  return data;
};
```

## 3. 例外を返す

:::message
エラー処理を自作するため、開発者が求めている情報が得られやすい
:::
:::message alert
自作する為、開発コスト高い
:::

```ts
/**
 * @throw {InvaildDateFormatError) ユーザーが誕生日を誤って入力した
 */

const parse = (birthday: string): Date | InvaildDateFormatError => {
  let data = new Date(birthday);
  if (isVaild(birthday)) {
    return new InvaildDateFormatError('Enter a data in the form YYYY/MM/DD');
  }
  return data;
};
```

## YouTube のご案内

ポモドーロタイマー（25 分勉強＋ 5 分休憩）を活用した作業・勉強配信を行っています。
集中したいときや、誰かと一緒に頑張りたいときに、ぜひご活用ください。

ご興味のある方は、ぜひお気軽に遊びに来てください！
「Zenn から来ました!!」とコメントを貰えると泣いて喜びます 🤣

@[card](https://www.youtube.com/@aew2sbee)
