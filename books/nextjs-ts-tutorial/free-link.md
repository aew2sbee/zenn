---
title: '[知識] ページ遷移(Link コンポーネント)'
free: true
---

## Link コンポーネントとは

> クライアントサイドでのナビゲーションをサポートするための特別なコンポーネントです。これを使用すると、ページ間のシームレスな遷移を実現できます。

## ページ遷移の実装

### 1. Link コンポーネント

```tsx
// Linkコンポーネントをインポート
import Link from 'next/link';

// hrefにページ遷移先を指定する
<Link href="/ssg">Go to SSG Page</Link>;
```

:::message
`http://localhost:3000/`の`Go to SSG Page`をクリックすると、<br>`http://localhost:3000/ssg`に遷移する
:::

![images/books/nextjs-ts-tutorial/link/nextjs-link-step01.png](/images/books/nextjs-ts-tutorial/link/nextjs-link-step01.png)

### 2. Link コンポーネント with クエリパラメータ付き

```tsx
// Linkコンポーネントをインポート
import Link from 'next/link';

// オブジェクトのpathnameにページ遷移先を指定する
// オブジェクトのqueryにクエリパラメータを指定する
<Link
  href={{
    pathname: '/ssg',
    query: { keyword: 'hello' },
  }}
>
  Go to SSG Page with query
</Link>;
```

:::message
`http://localhost:3000/`の`Go to SSG Page with query`をクリックすると、<br>`http://localhost:3000/ssg?keyword=hello`に遷移する
:::

![images/books/nextjs-ts-tutorial/link/nextjs-link-step02.png](/images/books/nextjs-ts-tutorial/link/nextjs-link-step02.png)

### 3. Link コンポーネント with button タグ

```tsx
// Linkコンポーネントをインポート
import Link from 'next/link';

// buttonがonClickされたら、ページ遷移する
<Link href="/ssg">
  <button>Jump to SSG Page</button>
</Link>;
```

:::message
`http://localhost:3000/`の`Jump to SSG Page`をクリックすると、<br>`http://localhost:3000/ssg`に遷移する
:::

![images/books/nextjs-ts-tutorial/link/nextjs-link-step03.png](/images/books/nextjs-ts-tutorial/link/nextjs-link-step03.png)
