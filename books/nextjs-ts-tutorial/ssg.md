---
title: '[知識] 静的サイト生成(SSG)'
---

## SSG (Static Site Generation)とは
> ページがビルド時に生成され、リクエストがあったときにはサーバーで動的にデータを取得するのではなく、事前に生成された静的なHTMLファイルが返される仕組みです。これにより、パフォーマンスが向上し、ユーザーエクスペリエンスが向上します。

### SSGのイメージ

![next.js-SSG](/images/next.js-SSG.png)

### SSGの流れ
1. build時に`getStaticProps`がページに必要なデータをAPI等で取得し、`props`を返す
2. `props`を各コンポーネントに渡してページを作成する

## SSGによるページの実装
```tsx
import * as next from 'next'
import Head from 'next/head'
import Link from 'next/link'

type SSGprops = {}


const SSG: next.NextPages<SSGprops> = {} => {
  return (
    <>
      <Head>
        <titile>Static Site Generation</titile>
        <Link>rel='icon' href="/favicon.ico</Link>
      </Head>
      <main>
        <p>This page is generated using Static Site Generation (SSG).</p>
      </main>
    </>
  )
}

export default SSG
```