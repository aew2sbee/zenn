---
title: '静的サイト生成(SSG)'
---

## SSG (Static Site Generation)とは
> ページがビルド時に生成され、リクエストがあったときにはサーバーで動的にデータを取得するのではなく、事前に生成された静的なHTMLファイルが返される仕組みです。これにより、パフォーマンスが向上し、ユーザーエクスペリエンスが向上します。

### SSGのイメージ

![next.js-SSG-step01](/images/next.js-SSG-step01.png)

### SSGの流れ
1. build時に`getStaticProps`がページに必要なデータをAPI等で取得し、`props`を返す
2. `props`を各コンポーネントに渡してページを作成する

## SSGによるページの実装 [単体ページ]
1. `frontend/src/pages/ssg.tsx`を新規で作成する
2. `frontend/src/pages/ssg.tsx`の中身を下記のコードにする
    ```tsx: frontend/src/pages/ssg.tsx
    import * as next from 'next';
    import Head from 'next/head';
    import Link from 'next/link';

    type SSGProps = {
      message: string;
    };

    const SSG: next.NextPage<SSGProps> = (props) => {
      const { message } = props;
      return (
        <>
          <Head>
            <title>Static Site Generation</title>
            <Link rel="icon" href="/favicon.ico" />
          </Head>
          <main>
            <p>This page is generated using Static Site Generation (SSG).</p>
            <p>{message}</p>
          </main>
        </>
      );
    };

    export const getStaticProps: next.GetStaticProps<SSGProps> = async (
      context
    ) => {
      const timestamp = new Date().toLocaleString();
      const message = `[${timestamp}] : The getStaticProps was executed`;
      console.log(message);
      return {
        props: { message },
      };
    };

    export default SSG;

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
      Generating static pages (0/6)  [=== ][2023/12/14 11:12:53] : The getStaticProps was executed
    ✓ Generating static pages (6/6) 
    ✓ Collecting build traces    
    ✓ Finalizing page optimization

    Route (app)                                Size     First Load JS
    ┌ ○ /                                      5.13 kB          87 kB
    └ ○ /_not-found                            875 B          82.7 kB
    + First Load JS shared by all              81.9 kB
      ├ chunks/938-5e061ba0d46125b1.js         26.7 kB
      ├ chunks/fd9d1056-735d320b4b8745cb.js    53.3 kB
      ├ chunks/main-app-2bd572ae84471028.js    220 B
      └ chunks/webpack-b527e254b83efc40.js     1.68 kB

    Route (pages)                              Size     First Load JS
    ─ ● /ssg (329 ms)                          2.7 kB         81.4 kB
    + First Load JS shared by all              78.7 kB
      ├ chunks/framework-b7ba9a8e7304c68b.js   45.2 kB
      ├ chunks/main-d6b31e3cb47a03bc.js        31.6 kB
      ├ chunks/pages/_app-98cb51ec6f9f135f.js  195 B
      └ chunks/webpack-b527e254b83efc40.js     1.68 kB

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

    ✓ Ready in 5.1s
    ○ Compiling /ssg ...
    ✓ Compiled /ssg in 7.9s (617 modules)
    ✓ Compiled (603 modules)
    [2023/12/14 11:14:01] : The getStaticProps was executed
    ⚠ Fast Refresh had to perform a full reload. Read more: https://nextjs.org/docs/messages/fast-refresh-reload
    ✓ Compiled /favicon.ico in 227ms (297 modules)
    ✓ Compiled in 328ms (645 modules)
    [2023/12/14 11:14:16] : The getStaticProps was executed
    [2023/12/14 11:14:23] : The getStaticProps was executed
    [2023/12/14 11:35:22] : The getStaticProps was executed
    ```
5. [SSG実装ページ](http://localhost:3000/sgg)にアクセスする
    下記の画像のようなページが表示される
    :::message
    ページアクセス時に最新コードで`getStaticProps`が実行されるため、時刻が更新される
    [2023/12/14 11:12:53] -> new [2023/12/14 11:35:22]
    :::
    ![next.js-SSG-step02](/images/next.js-SSG-step02.png)

## SSGによるページの実装 [複数ページ]
1. `frontend/src/pages/books/[id].tsx`を新規で作成する
2. `frontend/src/pages/books/[id].tsx`の中身を下記のコードにする
    ```tsx: frontend/src/pages/books/[id].tsx
    import * as next from 'next'
    import Head from 'next/head'
    import Link from 'next/link'
    import { useRouter } from 'next/router'
    import { ParsedUrlQuery } from 'querystring'

    type BookProps = {
      id: string | undefined
    }

    const Book: next.NextPage<BookProps> = (props) => {
      const { id } = props
      const router = useRouter()

      if (router.isFallback) {
        return <>Loading....</>
      }

      return (
        <>
          <Head>
            <title>Static Site Generation</title>
            <Link rel="icon" href="/favicon.ico" />
          </Head>
          <main>
            <p>This page was generated using Static Site Generation (SSG).</p>
            <p>{`This URL of page was /books/${id}`}</p>
          </main>
        </>
      )
    }

    export const getStaticPaths: next.GetStaticPaths = async () => {
      const paths = [
        { params: { id: '1' }},
        { params: { id: '2' }},
        { params: { id: '3' }}
      ]
      return { paths, fallback: false }
    }

    // パラメータの型を定義
    interface BookParams extends ParsedUrlQuery {
      id: string
    }

    export const getStaticProps: next.GetStaticProps<BookProps, BookParams> = async (context) => {
      return {
        props: { id: context.params!['id'] }
      }
    }

    export default Book

    ```
3. buildコマンドを実行して`getStaticPaths`を実行する
    ```bash
    npm run build
    ```
    ```bash
    $ npm run build

    > frontend@0.1.0 build
    > next build

      ▲ Next.js 14.0.4

    ✓ Creating an optimized production build    
    ✓ Compiled successfully
    ✓ Linting and checking validity of types
    ✓ Collecting page data    
      Generating static pages (3/9)  [=== ]
    ✓ Generating static pages (9/9)
    ✓ Collecting build traces    
    ✓ Finalizing page optimization

    Route (app)                                Size     First Load JS
    ┌ ○ /                                      5.13 kB          87 kB
    └ ○ /_not-found                            875 B          82.7 kB
    + First Load JS shared by all              81.9 kB
      ├ chunks/938-5e061ba0d46125b1.js         26.7 kB
      ├ chunks/fd9d1056-735d320b4b8745cb.js    53.3 kB
      ├ chunks/main-app-2bd572ae84471028.js    220 B
      └ chunks/webpack-8475e3af15ed9b2f.js     1.68 kB

    Route (pages)                              Size     First Load JS
    ┌ ● /books/[id] (968 ms)                   496 B          81.6 kB
    ├   ├ /books/2 (348 ms)
    ├   ├ /books/1 (330 ms)
    ├   └ /books/3
    └ ● /ssg (326 ms)                          418 B          81.6 kB
    + First Load JS shared by all              78.7 kB
      ├ chunks/framework-b7ba9a8e7304c68b.js   45.2 kB
      ├ chunks/main-d6b31e3cb47a03bc.js        31.6 kB
      ├ chunks/pages/_app-98cb51ec6f9f135f.js  195 B
      └ chunks/webpack-8475e3af15ed9b2f.js     1.68 kB

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

    ✓ Ready in 5.3s
    ○ Compiling /books/[id] ...
    ✓ Compiled /books/[id] in 1899ms (268 modules)
    ```
5. [SSG実装ページ](http://localhost:3000/books/1)にアクセスする
    下記の画像のようなページが表示される
    ![next.js-SSG-step03](/images/next.js-SSG-step03.png)
