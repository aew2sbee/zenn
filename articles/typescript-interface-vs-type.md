---
title: '[TypeScript] インターフェース(interface)と型エイリアス(type)の違い' # 記事のタイトル
emoji: '🛡' # アイキャッチとして使われる絵文字（1文字だけ）
type: 'tech' # tech: 技術記事 / idea: アイデア記事
topics: ['typescript', '初心者向け'] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開設定（falseにすると下書き）
---

## はじめに

`TypeScript`をより深く理解したく下記書籍を読みました。
`インターフェース(interface)と型エイリアス(type)の違い`について情報を整理したかったので、執筆します。
@[card](https://www.oreilly.co.jp/books/9784814400362/)

## 結論

:::message
`interface`の方が優れているので、`interface`の利用を推奨します
:::

## 1. interface 同士で継承できる

- 組み込みグローバルインターフェースや npm パッケージなどのサードパーティコードを扱う時に便利

```ts
interface 継承元 {
  変数名1: データ型;
}

interface 継承先 extends 継承元 {
  変数名2: データ型;
}
```

## 2. クラス宣言の構造の型チェックに

- クラス宣言の構造の型チェックに interface が利用できます
- 型エイリアス(type)ではクラス宣言では使えません。

## 3. interface の方が処理が速い

- interface は、内部的に簡単にキャッシュできるので一般的に型チェックでより高速に処理される
- 型エイリアス(type)は、オブジェクトリテラルの動的なコピペとして処理されるため、interface より遅い

## YouTube のご案内

ポモドーロタイマー（25 分勉強＋ 5 分休憩）を活用した作業・勉強配信を行っています。
集中したいときや、誰かと一緒に頑張りたいときに、ぜひご活用ください。

ご興味のある方は、ぜひお気軽に遊びに来てください！
「Zenn から来ました!!」とコメントを貰えると泣いて喜びます 🤣

@[card](https://www.youtube.com/@aew2sbee)
