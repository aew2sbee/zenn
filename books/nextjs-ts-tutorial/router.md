---
title: '[知識] ページ遷移(routerオブジェクト)'
---

## routerオブジェクトとは

> 現在のページやリダイレクト、クエリパラメータ、ルートなど、アプリケーション内でのナビゲーションに関する情報を提供するためのものです。

## ページ遷移の実装

### 1. ページ遷移[push]

```tsx
// useRouterをインポート
import { useRouter } from 'next/router';

const router = useRouter();
// pushの引数にページ遷移先を指定する
<button onClick={() => router.push("/ssg")}>Go to SSG Page</button>
```
:::message
`http://localhost:3000/`の`Go to SSG Page`をクリックすると、<br>`http://localhost:3000/ssg`に遷移する
:::

![nextjs-router-step01](/images/books/nextjs-ts-tutorial/router/nextjs-router-step01.png)


### 2. ページ遷移[reload]
```tsx
// useRouterをインポート
import { useRouter } from 'next/router';

const router = useRouter();
// 引数の指定は不要
<button onClick={() => router.reload()}>reload</button>
```

:::message
`http://localhost:3000/`の`reload`をクリックすると、<br>`http://localhost:3000/`に遷移し時間が更新する
:::

![nextjs-router-step02](/images/books/nextjs-ts-tutorial/router/nextjs-router-step02.png)

### 3. ページ遷移[back]

```tsx
// routerをインポート
import { useRouter } from 'next/router';

const router = useRouter();
// hrefにページ遷移先を指定する
<button onClick={() => router.back()}>back</button>
```

:::message
`http://localhost:3000/ssg`の`back`をクリックすると、<br>`http://localhost:3000/`に遷移する
:::

![nextjs-router-step03](/images/books/nextjs-ts-tutorial/router/nextjs-router-step03.png)