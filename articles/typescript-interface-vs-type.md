---
title: '[TypeScript] インターフェース(interface)と型エイリアス(type)の違い' # 記事のタイトル
emoji: '🐓' # アイキャッチとして使われる絵文字（1文字だけ）
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

## 1. interface同士で継承できる
- 組み込みグローバルインターフェースやnpmパッケージなどのサードパーティコードを扱う時に便利
```ts
interface 継承元 {
  変数名1: データ型;
}

interface 継承先 extends 継承元 {
  変数名2: データ型;
}
```

## 2. クラス宣言の構造の型チェックに
- クラス宣言の構造の型チェックにinterfaceが利用できます
- 型エイリアス(type)ではクラス宣言では使えません。

## 3. interfaceの方が処理が速い
- interfaceは、内部的に簡単にキャッシュできるので一般的に型チェックでより高速に処理される
- 型エイリアス(type)は、オブジェクトリテラルの動的なコピペとして処理されるため、interfaceより遅い
