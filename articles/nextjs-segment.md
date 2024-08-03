---
title: '[Next.js] Segment構成ファイル' # 記事のタイトル
emoji: '🕊' # アイキャッチとして使われる絵文字（1文字だけ）
type: 'tech' # tech: 技術記事 / idea: アイデア記事
topics: ['nextjs', '初心者向け'] # タグ。["markdown", "rust", "aws"]のように指定する
published: false # 公開設定（falseにすると下書き）
---

## はじめに 
今回は下記書籍でNext.jsについて学習しました。
下記内容まで解説します。

:::message
**Segment構成ファイルの解説**
:::


@[card](https://amzn.asia/d/3oB63FN)

### 結論

| 名称 | 拡張子 | 用途 |
| ---- | ---- | ---- |
| layout | .js .jsx .tsx | Segmentとその子の共有UI |
| page | .js .jsx .tsx | Segment独自のUI |
| loading | .js .jsx .tsx | Segmentとその子の読み込みUI |
| not-found | .js .jsx .tsx | Segmentとその子の404UI |
| error | .js .jsx .tsx | Segmentとその子のエラーUI |
| route | .js .ts | サーバーサイドAPIエンドポイント |
| tempplate | .js .jsx .tsx | 再レンダリングされるレイアウトUI |
| default | .js .jsx .tsx | Parallel routeのフォールバックUI |

## 各構成ファイルの意味

### loading
- `<Suspense />`の上に構築できる簡易的なローディングUIを提供できる
- `Server Compponent/Client Compponent`ともに利用できる
- Propsは指摘できない

```tsx
export default function Loading() {
  return <>...loading</>;
}
```

### not-found
- `404ステータス`の時にUIを提供する
- 関連するSegmentから`notFound()`が実行されると呼び出される
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
- エラーが発生時に表示するUI
- `use client`を宣言し、Client Compponentとして定義する必要がある

```tsx
"use client";

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
- Web APIを提供するためのファイル

```ts
export async function GET(request: Request) {
  return new Response ('Hello Next.js!');
}
``` 
