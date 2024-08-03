---
title: '[Next.js] Server Componet とClient Componentとは？' # 記事のタイトル
emoji: '🍔' # アイキャッチとして使われる絵文字（1文字だけ）
type: 'tech' # tech: 技術記事 / idea: アイデア記事
topics: ['nextjs', 'フロントエンド', 'typescript', '初心者向け'] # タグ。["markdown", "rust", "aws"]のように指定する
published: false # 公開設定（falseにすると下書き）
---

## はじめに

今回は下記書籍で Next.js について学習しました。
下記内容まで解説します。

:::message
**Segment 構成ファイルの解説**
:::

@[card](https://amzn.asia/d/3oB63FN)

### 結論

|  Server Compoent を使うべきケース                                          | Client Compoent を使うべきケース                                                                                                                                                  |
| -------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| - データを取得する<br>- バックエンドリソースを取得する<br>- 機密情報を扱う | - インタラクティブな機能を持つ<br>- コンポーネントに保持した値を扱う<br>- ブラウザ専用の API を使用する<br>- ブラウザ専用の Hool を使用する<br>- React Class コンポーネントを使う |

## Server Componet(SRC)とは
> サーバーサイドのみ実装されるコンポーネント

:::message
**「非同期関数(async function)」** が使える
⇒直接外部WebAPIのデータを取得してレンダリングする事が可能
:::


:::message alert
何も宣言をしなければ、defaultですべてのコンポーネントがSRCとして扱われる
⇒ブラウザで実行すべきJavaScriptが送られない=イベントハンドラー等が出来ない
:::

```tsx
export default async function ServerComponent () {
  const res = await fetch('http://localhost:4000/posts');
  const posts = await res.json();
  return (
    <div>
      <ul>
        {posts.map((post) => (
          <li key={post.id}>{post.title}</li>
        ))}
      </ul>
    </div>
  );
}
```

## Client Componentとは
> ブラウザ/サーバー両方で実行されるコンポーネント
:::message
ブラウザで実行すべきJavaScriptが送れる=イベントハンドラー等が出来る
:::


:::message alert
ファイル冒頭に**"use client";** を記載する
⇒Client Componentとして扱われる

もし、親がClient Componentなら再宣言する必要がありません。
:::

```tsx
"use client"; 

import { useState } from 'react';

export default function Counter() {
  const [count, setCount] = useState(0);
  return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={() => setCount(count + 1)}>Click me</button>
    </div>
  );
}
```