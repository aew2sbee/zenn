---
title: "[Next.js] Server ComponentとClient Componentとは？" # 記事のタイトル
emoji: "⚡" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア記事
topics: ["nextjs", "フロントエンド", "typescript", "初心者向け"] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開:true / 非公開:false
---

## 🌱 はじめに

`Next.js`の学習のために、下記書籍を読みました。
下記書籍の`Server ComponentとClient Component`について学びがあったので、記事として記録します。

@[card](https://gihyo.jp/book/2024/978-4-297-14061-8)

:::message
本記事は、App Router（`app` ディレクトリ）を前提にしています。
:::

## 🌱 結論

:::message
**Server Component を使うべきケース**

- データを取得する
- バックエンドリソース（DB など）に直接アクセスする
- 機密情報を扱う

---

**Client Component を使うべきケース**

- インタラクティブな機能を持つ
- state やライフサイクル（`useState`/`useEffect` など）を使う
- ブラウザ専用の API を使用する
- state やエフェクトに依存するフックを使用する
- React Class コンポーネントを使う

:::

## 🌱 Server Component(RSC)とは

> サーバー上（リクエスト時またはビルド時）でのみ実行されるコンポーネント

:::message
**「非同期関数(async function)」** が使える
⇒ 直接外部 WebAPI のデータを取得してレンダリングすることが可能
:::

:::message alert
App Router では、何も宣言をしなければ、デフォルトですべてのコンポーネントが RSC として扱われる
⇒ ブラウザで実行すべき JavaScript が送られない=イベントハンドラ等が使えない
:::

下記は、ローカルで 4000 番ポートに API サーバー（json-server など）を起動している前提の例です。

```tsx
type Post = { id: number; title: string };

export default async function ServerComponent() {
  const res = await fetch("http://localhost:4000/posts");
  if (!res.ok) throw new Error("Failed to fetch posts");
  const posts: Post[] = await res.json();
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

## 🌱 Client Component とは

> ブラウザ/サーバー両方で実行されるコンポーネント

:::message
ブラウザで実行すべき JavaScript が送れる=イベントハンドラ等が使える
:::

:::message alert
ファイル冒頭に **"use client";** を記載する
⇒Client Component として扱われる

`"use client"` を記載したファイルから import されるコンポーネントは、再宣言しなくても Client Component として扱われる（境界は import 関係で決まる）。
一方、Server Component から `children` として渡したコンポーネントは、Server Component のまま。
:::

```tsx
"use client";

import { useState } from "react";

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
