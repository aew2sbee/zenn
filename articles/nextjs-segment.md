---
title: '[Next.js] Segment構成ファイル' # 記事のタイトル
emoji: '🕊' # アイキャッチとして使われる絵文字（1文字だけ）
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

| 名称      | 拡張子        | 用途                               |
| --------- | ------------- | ---------------------------------- |
| layout    | .js .jsx .tsx | Segment とその子の共有 UI          |
| page      | .js .jsx .tsx | Segment 独自の UI                  |
| loading   | .js .jsx .tsx | Segment とその子の読み込み UI      |
| not-found | .js .jsx .tsx | Segment とその子の 404UI           |
| error     | .js .jsx .tsx | Segment とその子のエラー UI        |
| route     | .js .ts       | サーバーサイド API エンドポイント  |
| tempplate | .js .jsx .tsx | 再レンダリングされるレイアウト UI  |
| default   | .js .jsx .tsx | Parallel route のフォールバック UI |

## 各構成ファイルの意味

### loading

- `<Suspense />`の上に構築できる簡易的なローディング UI を提供できる
- `Server Compponent/Client Compponent`ともに利用できる
- Props は指摘できない

```tsx
export default function Loading() {
  return <>...loading</>;
}
```

### not-found

- `404ステータス`の時に UI を提供する
- 関連する Segment から`notFound()`が実行されると呼び出される

```tsx
export default async function NotFound() {
  return <div>404: Not Found</div>;
}
```

```tsx
import { notFound } from "next/navigation";

export default async function Page({ searchParams }: Props) {
  const page = getPage(searchParams);
  if (page > 10) {
    // 11ページ以降は404扱いにする
    notFound();
  }
```

### error

- エラーが発生時に表示する UI
- `use client`を宣言し、Client Compponent として定義する必要がある

```tsx
'use client';

export default function GlobalError({
  reset,
}: {
  error: Error & { digest?: string };
  reset: () => void;
}) {
  return (
    <html>
      <body>
        <h2>Something went wrong!</h2>
        <button onClick={() => reset()}>Try again</button>
      </body>
    </html>
  );
}
```

### route

- Web API を提供するためのファイル

```ts
export async function GET(request: Request) {
  return new Response('Hello Next.js!');
}
```
