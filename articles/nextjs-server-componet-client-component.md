---
title: '[Next.js] Server Componet とClient Componentとは？' # 記事のタイトル
emoji: '⚡' # アイキャッチとして使われる絵文字（1文字だけ）
type: 'tech' # tech: 技術記事 / idea: アイデア記事
topics: ['nextjs', 'フロントエンド', 'typescript', '初心者向け'] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開設定（falseにすると下書き）
---

## はじめに

`Next.js`の学習の為に、下記書籍を読みました。
下記書籍の`Server Componet とClient Component`について学びがあったので、記事として記録します。
@[card](https://gihyo.jp/book/2024/978-4-297-14061-8)

## 結論

:::message
**Server Compoent を使うべきケース**

- データを取得する
- バックエンドリソースを取得する
- 機密情報を扱う

---

**Client Compoent を使うべきケース**

- インタラクティブな機能を持つ
- コンポーネントに保持した値を扱う
- ブラウザ専用の API を使用する
- ブラウザ専用の Hool を使用する
- React Class コンポーネントを使う

:::

## Server Componet(SRC)とは

> サーバーサイドのみ実装されるコンポーネント

:::message
**「非同期関数(async function)」** が使える
⇒ 直接外部 WebAPI のデータを取得してレンダリングする事が可能
:::

:::message alert
何も宣言をしなければ、default ですべてのコンポーネントが SRC として扱われる
⇒ ブラウザで実行すべき JavaScript が送られない=イベントハンドラー等が出来ない
:::

```tsx
export default async function ServerComponent() {
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

## Client Component とは

> ブラウザ/サーバー両方で実行されるコンポーネント
> :::message
> ブラウザで実行すべき JavaScript が送れる=イベントハンドラー等が出来る
> :::

:::message alert
ファイル冒頭に **"use client";** を記載する
⇒Client Component として扱われる

もし、親が Client Component なら再宣言する必要がありません。
:::

```tsx
'use client';

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

## YouTube のご案内

ポモドーロタイマー（25 分勉強＋ 5 分休憩）を活用した作業・勉強配信を行っています。
集中したいときや、誰かと一緒に頑張りたいときに、ぜひご活用ください。

ご興味のある方は、ぜひお気軽に遊びに来てください！

@[card](https://www.youtube.com/@aew2sbee)
