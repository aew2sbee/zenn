---
title: '[XSS] リンクのhrefに起因するDOM Based XSSを防ぐ方法' # 記事のタイトル
emoji: '⚠️' # アイキャッチとして使われる絵文字（1文字だけ）
type: 'tech' # tech: 技術記事 / idea: アイデア記事
topics: ['xss', 'javascript', 'security', '初心者向け'] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開:true / 非公開:false
---

## 🌱 はじめに

2023年ごろに下記の書籍を読みましたが、あまり深く理解できませんでした。
2025年に`情報セキュリティマネジメント試験`、`基本情報技術者試験`に合格し、基礎知識が身につきました。
改めて書籍を読み直し、アウトプットを行うことで理解を深めていこうと思います。

https://www.shoeisha.co.jp/book/detail/9784798169477

本記事では、ユーザーが入力したURLを`<a>`タグの`href`に設定するケースに限定して解説します。

## 🌱 結論

:::message
`<a>`タグの`href`にユーザーの入力値をセットする前に、`URL.protocol`が`http:`または`https:`であることを検証し、**DOM型クロスサイトスクリプティング**を防ぎます。

```diff html
   <script>
     const url = new URL(window.location.href);
     const urlStr = url.searchParams.get('url');
     if (urlStr) {
       const linkUrl = new URL(urlStr, url.origin);
-      document.querySelector('#link').href = linkUrl;
+      if (linkUrl.protocol === 'http:' || linkUrl.protocol === 'https:') {
+        document.querySelector('#link').href = linkUrl;
+      } else {
+        console.warn('危険なURLが入力されました: ' + linkUrl);
+      }
     }
   </script>
```

:::

:::message
**ポイント**

- 入力値の文字列を`startsWith('javascript:')`のように比較するのではなく、URLパーサー（`new URL()`）で解析した後の`protocol`を、許可するスキームのリストと比較します。
- URLパーサーは、スキームの大文字を小文字にし、先頭の空白や途中のタブ・改行を取り除きます。そのため、`JavaScript:`や、途中にタブを挟んだ`java（タブ）script:`のような書き方でも`javascript:`として判定でき、検証をすり抜けられません。
:::

## 🌱 検証

1. DOM型クロスサイトスクリプティングの脆弱性があるHTMLファイルを作成し、`xss.html`として保存します。

   ```html:xss.html
   <!doctype html>
   <html lang="ja">
     <head>
       <meta charset="UTF-8" />
       <title>XSS検証用ページ</title>
     </head>
     <body>
       <h1>XSS検証用ページ</h1>
       <div id="result"></div>
       <a id="link" href="#">ここをクリックしてXSS攻撃を試す</a>
       <script>
         const url = new URL(window.location.href);
         const urlStr = url.searchParams.get('url');
         if (urlStr) {
           const linkUrl = new URL(urlStr, url.origin);
           document.querySelector('#link').href = linkUrl;
         }
       </script>
     </body>
   </html>
   ```

2. `xss.html`を置いたディレクトリで、ポート3001でHTMLファイルを配信します（下記はPythonを使う例）。

   ```bash
   python -m http.server 3001
   ```

   :::message alert
   HTMLファイルをブラウザで直接開く（`file://`）と、`url.origin`が`"null"`になり、`new URL()`がエラーになります。必ずHTTPサーバー経由で開いてください。
   :::

3. 下記URLにアクセスし、`ここをクリックしてXSS攻撃を試す`のリンクをクリックします。

   ```text
   http://localhost:3001/xss.html?url=javascript:alert('xss by javascript')
   ```

   URLパラメータに含まれる`javascript:`スキームのコードが実行されたことが確認できます。

   ![before](/images/articles/http-cross-sitescripting-link/before.png)

4. DOM型クロスサイトスクリプティングを防ぐため、`protocol`が`http:`または`https:`であるかを確認するコードに修正します。

   ```diff html:xss.html
    <!doctype html>
    <html lang="ja">
      <head>
        <meta charset="UTF-8" />
        <title>XSS検証用ページ</title>
      </head>
      <body>
        <h1>XSS検証用ページ</h1>
        <div id="result"></div>
        <a id="link" href="#">ここをクリックしてXSS攻撃を試す</a>
        <script>
          const url = new URL(window.location.href);
          const urlStr = url.searchParams.get('url');
          if (urlStr) {
            const linkUrl = new URL(urlStr, url.origin);
   -        document.querySelector('#link').href = linkUrl;
   +        if (linkUrl.protocol === 'http:' || linkUrl.protocol === 'https:') {
   +          document.querySelector('#link').href = linkUrl;
   +        } else {
   +          console.warn('危険なURLが入力されました: ' + linkUrl);
   +        }
          }
        </script>
      </body>
    </html>
   ```

5. 再び、下記URLにアクセスし、`ここをクリックしてXSS攻撃を試す`のリンクをクリックします。

   ```text
   http://localhost:3001/xss.html?url=javascript:alert('xss by javascript')
   ```

   `javascript:`スキームのコードが実行されず、ブラウザの開発者ツールのコンソールに警告が出力されたことが確認できます。

   ![after](/images/articles/http-cross-sitescripting-link/after.png)

## 🌱 補足

:::message alert
この対策で防げるのは、`javascript:`スキームなどによるスクリプトの実行です。
`https://evil.example/`や`//evil.example/`のような外部サイトのURLは検証を通るため、外部サイトへの誘導（オープンリダイレクト）は防げません。防ぐ場合は、`linkUrl.origin === location.origin`や、許可するホストのリストでの確認を組み合わせてください。
:::

- `http://`のようにURLとして不正な値を渡すと、`new URL()`がエラーになり、以降の処理が止まります。実際のアプリケーションでは`try...catch`で囲むとよいです。
- 多層防御として、CSP（Content Security Policy）で`script-src`に`'unsafe-inline'`を許可しなければ、`javascript:`スキームのURLの実行もブロックできます。
