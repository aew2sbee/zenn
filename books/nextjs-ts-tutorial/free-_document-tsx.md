---
title: '[作業] _document.tsxの設定'
---

## _document.tsxとは
> Next.js フレームワークで使用される特殊なファイルの一部です。<br>ページがサーバーサイドでレンダリングされる際に HTML の全体構造を定義します。通常、このファイルはカスタムな HTML マークアップやグローバルな設定を行うために使われます。

## _document.tsxの作成
下記コマンドでデフォルトで作成する事が出来ますが、PJ作成時に作成されます。
````bash
cd frontend
touch src/pages/_document.tsx

````

## _document.tsxの更新
````tsx
import Document, { DocumentContext, DocumentInitialProps } from 'next/document'
// ServerStyleSheet: サーバーサイドでのスタイルの収集を可能にする styled-components のユーティリティクラスです。
import { ServerStyleSheet } from 'styled-components'

export default class MyDocument extends Document {
  static async getInitialProps(
    ctx: DocumentContext,
  ): Promise<DocumentInitialProps> {
    // ServerStyleSheet インスタンスを作成
    const sheet = new ServerStyleSheet()
    // 初期の renderPage メソッドを保存
    const originalRenderPage = ctx.renderPage

    try {
      // renderPage メソッドを上書きして、スタイルを収集するように設定
      ctx.renderPage = () =>
        originalRenderPage({
          enhanceApp: (App) => (props) =>
            sheet.collectStyles(<App {...props} />),
        })

      // Document.getInitialProps を呼び出して初期プロパティを取得、
      // サーバーサイドでの初期化時に呼び出され、レンダリングに関連する情報を取得します
      const initialProps = await Document.getInitialProps(ctx)

      // 収集したスタイルを初期プロパティに追加
      return {
        ...initialProps,
        // 収集したスタイルを初期プロパティに追加、サーバーサイドでのスタイルが適切にクライアントに伝える。
        styles: [
          <>
            {initialProps.styles}
            {sheet.getStyleElement()}
          </>,
        ],
      }
    } finally {
      // スタイルシートをシールし、メモリリークを防ぐ
      sheet.seal()
    }
  }
}

````