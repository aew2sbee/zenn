---
title: '[知識] インクリメント静的サイト生成(ISR)'
free: true
---

## ISR (Incremental Static Regeneration)とは
> ページの一部を静的生成し、一部を動的に再生成することで、最適なパフォーマンスと最新のデータを提供する機能です

### ISRの流れ
1. build時に`getStaticProps`がページに必要なデータをAPI等で取得し、`props`を返す
2. `props`を各コンポーネントに渡してページを作成する
3. `redirect`で指定された秒数ごとにページが再生成される

## ISRによるページの実装 [単体ページ]
1. `frontend/src/pages/isr.tsx`を新規で作成する
2. `frontend/src/pages/isr.tsx`の中身を下記のコードにする
    ```tsx: frontend/src/pages/isr.tsx
    import * as next from 'next'
    import Head from 'next/head'
    import Link from 'next/link'
    import { useRouter } from 'next/router'

    type ISRProps = {
      message: string
    }

    const ISR: next.NextPage<ISRProps> = (props) => {
      const { message } = props
      const router = useRouter()

      if (router.isFallback) {
        return <div>Loading...</div>
      }

      return (
        <>
          <Head>
            <title>Static Site Generation</title>
            <Link rel="icon" href="/favicon.ico" />
          </Head>
          <main>
            <p>This page was generated using Static Site Generation (ISR).</p>
            <p>{message}</p>
          </main>
        </>
      )
    }

    export const getStaticProps: next.GetStaticProps<ISRProps> = async (context) => {
      const timestamp = new Date().toLocaleString()
      const message = `[${timestamp}] : The getStaticProps was executed(ISR)`
      console.log(message)
      return {
        props: { message },
        // ページの有効期間を秒単位で指定
        redirect: 10
      }
    }

    export default ISR


    ```
3. buildコマンドを実行して`getStaticProps`を実行する
    ```bash
    npm run build
    ```
    :::message
    `Generating static pages`の時に
    事前に仕込んでおいたログが`[2023/12/14 11:12:53] : The getStaticProps was executed`が確認できる
    :::
    ```bash
    $ npm run build

    > frontend@0.1.0 build
    > next build

      ▲ Next.js 14.0.4

    ✓ Creating an optimized production build    
    ✓ Compiled successfully
    ✓ Linting and checking validity of types    
    ✓ Collecting page data    
      Generating static pages (0/10)  [    ][2023/12/27 16:18:27] : The getStaticProps was executed(ISR)
    [2023/12/27 16:18:27] : The getStaticProps was executed(SSG)
    ✓ Generating static pages (10/10)
    ✓ Collecting build traces
    ✓ Finalizing page optimization


    Route (pages)                              Size     First Load JS
    ┌ ○ /                                      414 B          81.3 kB
    ├ ○ /404                                   181 B          78.7 kB
    ├ ● /books/[id] (685 ms)                   481 B          81.4 kB
    ├   ├ /books/1
    ├   ├ /books/2
    ├   └ /books/3
    ├ ○ /image                                 4.22 kB        82.7 kB
    ├ ● /isr                                   466 B          81.4 kB
    └ ● /ssg                                   465 B          81.4 kB
    + First Load JS shared by all              78.5 kB
      ├ chunks/framework-b7ba9a8e7304c68b.js   45.2 kB
      ├ chunks/main-76e8a0b64b446374.js        31.6 kB
      ├ chunks/pages/_app-98cb51ec6f9f135f.js  195 B
      └ chunks/webpack-4fa19345907df07d.js     1.46 kB

    ○  (Static)  prerendered as static content
    ●  (SSG)     prerendered as static HTML (uses getStaticProps)
    ```

4. 実行コマンドを実行してローカルで起動する
    ```bash
    npm run dev
    ```

    ```bash
    ----- 出力結果 -----
    $ npm run dev

    > frontend@0.1.0 dev
    > next dev

      ▲ Next.js 14.0.4
      - Local:        http://localhost:3000

    ✓ Ready in 4.6s
    ○ Compiling / ...

    warn - No utility classes were detected in your source files. If this is unexpected, double-check the `content` option in your Tailwind CSS configuration.
    warn - https://tailwindcss.com/docs/content-configuration
    ✓ Compiled / in 7.1s (659 modules)
    ✓ Compiled /isr in 229ms (649 modules)
    [2023/12/27 16:19:19] : The getStaticProps was executed(ISR)
    ⚠ Fast Refresh had to perform a full reload. Read more: https://nextjs.org/docs/messages/fast-refresh-reload
    ⚠ Fast Refresh had to perform a full reload. Read more: https://nextjs.org/docs/messages/fast-refresh-reload
    ✓ Compiled /favicon.ico in 224ms (340 modules)
    ```
5. [ISR実装ページ](http://localhost:3000/isr)にアクセスする
    下記の画像のようなページが表示される
    :::message
    ページアクセス時に最新コードで`getStaticProps`が実行されるため、時刻が更新される
    [2023/12/14 11:12:53] -> new [2023/12/14 11:35:22]
    :::
    ![nextjs-isr-step01](/images/books/nextjs-ts-tutorial/isr/nextjs-isr-step01.png)
