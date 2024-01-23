---
title: '[知識] 環境変数'
free: true
---

## Next.jsの環境変数とは

|  | 説明 |
| ---- | ---- |
| 先頭`NEXT_PUBLIC_`なし | サーバーサイドのみ参照可能な変数 |
| 先頭`NEXT_PUBLIC_`あり | サーバーサイド/クライアントサイドの両方で参照可能な変数 |

:::message
**読み込みの優先順位**: .env < .env.local
:::


## 環境変数(.env)を取得するページの実装
1. `frontend/.env`を新規で作成する
    ```.env: frontend/.env
    TEST='TEST: サーバーサイドのみ参照可能な変数です'
    NEXT_PUBLIC_TEST='NEXT_PUBLIC_XXX: サーバーサイド/クライアントサイドの両方で参照可能な変数'
    ```
2. `frontend/src/pages/env.tsx`を新規で作成する
3. `frontend/src/pages/env.tsx`の中身を下記のコードにする
    ```tsx: frontend/src/pages/env.tsx
    import * as next from 'next'
    import Head from 'next/head'
    import Link from 'next/link'
    import { useRouter } from 'next/router'

    type ISRProps = {
      message: string
    }

    const ENV: next.NextPage = (props) => {
      console.log('ENV -> process.env.TEST', process.env.TEST)
      console.log('ENV -> process.env.NEXT_PUBLIC_TEST', process.env.NEXT_PUBLIC_TEST)

      return (
        <>
          <Head>
            <title>Static Site Generation</title>
            <Link rel="icon" href="/favicon.ico" />
          </Head>
          <main>
            <p>下記に`process.env.TEST`を表示しております。</p>
            <p>{process.env.TEST}</p>
            <p>下記に`process.env.NEXT_PUBLIC_TEST`を表示しております。</p>
            <p>{process.env.NEXT_PUBLIC_TEST}</p>
          </main>
        </>
      )
    }

    export const getStaticProps: next.GetStaticProps = async (context) => {
      console.log('getStaticProps -> process.env.TEST', process.env.TEST)
      console.log('getStaticProps -> process.env.NEXT_PUBLIC_TEST', process.env.NEXT_PUBLIC_TEST)
      return { props: {} }
    }

    export default ENV


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
      - Environments: .env

    ✓ Ready in 4.8s
    ○ Compiling / ...
    ✓ Compiled / in 1352ms (268 modules)
    ✓ Compiled /favicon.ico in 312ms (152 modules)
    ○ Compiling /env ...
    ✓ Compiled /env in 654ms (475 modules)
    getStaticProps -> process.env.TEST TEST: サーバーサイドのみ参照可能な変数です。
    getStaticProps -> process.env.NEXT_PUBLIC_TEST NEXT_PUBLIC_XXX: サーバーサイド/クライアントサイドの両方で参照可能な変数
    ENV -> process.env.TEST TEST: サーバーサイドのみ参照可能な変数です。
    ENV -> process.env.NEXT_PUBLIC_TEST NEXT_PUBLIC_XXX: サーバーサイド/クライアントサイドの両方で参照可能な変数

    ```
5. [.env実装ページ](http://localhost:3000/env)にアクセスする
    下記の画像のようなページが表示される
    ![nextjs-env-step01](/images/books/nextjs-ts-tutorial/env/nextjs-env-step01.png)
    ※下記画像の通りエラー画面が表示される為、右上の×ボタンで閉じます
    ![nextjs-env-step02](/images/books/nextjs-ts-tutorial/env/nextjs-env-step02.png)

## 環境変数(.env.local)を取得するページの実装
1. `frontend/.env.local`を新規で作成する
    ```.env: frontend/.env.local
    TEST='TEST: サーバーサイドのみ参照可能な変数(local)'
    NEXT_PUBLIC_TEST='NEXT_PUBLIC_XXX: サーバーサイド/クライアントサイドの両方で参照可能な変数(local)'
    ```
2. 実行コマンドを実行してローカルで起動する
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
      - Environments: .env.local, .env

    ✓ Ready in 4.1s
    ○ Compiling /not-found ...

    warn - No utility classes were detected in your source files. If this is unexpected, double-check the `content` option in your Tailwind CSS configuration.
    warn - https://tailwindcss.com/docs/content-configuration
    ✓ Compiled /not-found in 12.5s (619 modules)
    getStaticProps -> process.env.TEST TEST: サーバーサイドのみ参照可能な変数(local)
    getStaticProps -> process.env.NEXT_PUBLIC_TEST NEXT_PUBLIC_XXX: サーバーサイド/クライアントサイドの両方で参照可能な変数(local)
    ENV -> process.env.TEST TEST: サーバーサイドのみ参照可能な変数(local)
    ENV -> process.env.NEXT_PUBLIC_TEST NEXT_PUBLIC_XXX: サーバーサイド/クライアントサイドの両方で参照可能な変数(local)
    ⚠ Fast Refresh had to perform a full reload. Read more: https://nextjs.org/docs/messages/fast-refresh-reload
    getStaticProps -> process.env.TEST TEST: サーバーサイドのみ参照可能な変数(local)
    getStaticProps -> process.env.NEXT_PUBLIC_TEST NEXT_PUBLIC_XXX: サーバーサイド/クライアントサイドの両方で参照可能な変数(local)
    ENV -> process.env.TEST TEST: サーバーサイドのみ参照可能な変数(local)
    ENV -> process.env.NEXT_PUBLIC_TEST NEXT_PUBLIC_XXX: サーバーサイド/クライアントサイドの両方で参照可能な変数(local)
    getStaticProps -> process.env.TEST TEST: サーバーサイドのみ参照可能な変数(local)
    getStaticProps -> process.env.NEXT_PUBLIC_TEST NEXT_PUBLIC_XXX: サーバーサイド/クライアントサイドの両方で参照可能な変数(local)
    ENV -> process.env.TEST TEST: サーバーサイドのみ参照可能な変数(local)
    ENV -> process.env.NEXT_PUBLIC_TEST NEXT_PUBLIC_XXX: サーバーサイド/クライアントサイドの両方で参照可能な変数(local)
    ✓ Compiled /favicon.ico in 391ms (297 modules)
    ```
5. [.env実装ページ](http://localhost:3000/env)にアクセスする
    下記の画像のようなページが表示される
    ![nextjs-env-step03](/images/books/nextjs-ts-tutorial/env/nextjs-env-step03.png)