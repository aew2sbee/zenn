---
title: '[XSS] URLパラメータは`innerHTML`ではなく`textContent`で処理する' # 記事のタイトル
emoji: '⚠️' # アイキャッチとして使われる絵文字（1文字だけ）
type: 'tech' # tech: 技術記事 / idea: アイデア記事
topics: ['xss', 'javascript', 'security', '初心者向け'] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開:true / 非公開:false
---

## 🌱 はじめに

2023年ごろに下記の書籍を読みましたが、あまり深く理解できませんでした。
2025年に`情報セキュリティマネジメント試験`、`基本情報技術者試験`に合格し、基礎知識が身につきました。
改めて書籍を読み直し、アウトプットを行って理解を深めていこうと思います。

https://www.shoeisha.co.jp/book/detail/9784798169477

## 🌱 結論

:::message
URLパラメータなどの値を画面に表示するときは、`innerHTML`ではなく`textContent`を使います。
`textContent`は値をただの文字列として扱うため、値に含まれるタグやイベントハンドラがHTMLとして解釈されず、埋め込まれたJavaScriptの実行を防げます。

```diff html
-  document.getElementById('result').innerHTML = message;
+  document.getElementById('result').textContent = message;
```

:::

:::message alert
`textContent`で防げるのは、値をHTMLとして解釈させる処理（`innerHTML`など）に渡す場合だけです。
`a.href = message`や`location.href = message`、`eval(message)`のように値を渡す場合は、`textContent`では防げません。URLのスキーム（`http:`/`https:`）を検証するなど、別の対策が必要です。
:::

## 🌱 DOM-based XSSとは

DOM-based XSSは、ブラウザ上のJavaScriptが、URLなど攻撃者が操作できる値を`innerHTML`などの危険な処理に渡すことで発生するXSSです。
サーバーを経由せず、ブラウザ上だけで発生するため、クライアント側のコードで対策します。

## 🌱 検証

1. DOM-based XSSの脆弱性があるHTMLファイルを作成し、`xss.html`として保存する

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

       <script>
         const url = new URL(window.location.href);
         const message = url.searchParams.get('message');
         if (message) {
           document.getElementById('result').innerHTML = message;
         }
       </script>
     </body>
   </html>
   ```

2. `xss.html`を置いたディレクトリで、ポート3001でHTMLファイルを配信する（下記はPythonを使う例）

   ```bash
   python -m http.server 3001
   ```

3. ブラウザで下記URLを開き、URLに含まれているJavaScript（`alert`）が実行されることを確認する

   ```text
   http://localhost:3001/xss.html?message=<img%20src%20onerror=alert('xss')>
   ```

:::message
**ポイント**

- `%20`は、URLエンコーディング（パーセントエンコーディング）で半角スペースを表す
- `innerHTML`で挿入した`<script>`タグは実行されないため、`<img>`タグの`onerror`属性を使ってJavaScriptを実行させている
- `src`が空のため画像の読み込みが失敗し、`onerror`に書いた`alert('xss')`が実行される
:::

![before](/images/articles/http-cross-sitescripting/before.png)
_`alert`が実行されている様子_

:::message
画像には、本記事のHTMLにはないリンクが表示されていますが、検証結果には影響しません。
:::

4. XSS対策をする

   `innerHTML`は、代入した文字列をHTMLとして解析するため、タグやイベントハンドラが有効になります。
   `textContent`は、文字列をそのままテキストとして扱うため、タグとして解釈されません。
   そこで、URLパラメータをHTMLとして解釈させず、ただの文字列として扱うように変更する。

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

        <script>
          const url = new URL(window.location.href);
          const message = url.searchParams.get('message');
          if (message) {
   -        document.getElementById('result').innerHTML = message;
   +        document.getElementById('result').textContent = message;
          }
        </script>
      </body>
    </html>
   ```

5. 同じURLを開き直し、`alert`が実行されず、値が文字列として表示されることを確認する

![after](/images/articles/http-cross-sitescripting/after.png)
_`alert`が実行されなくなった様子_

## 🌱 おわりに

装飾付きのHTMLを表示したいなど、`textContent`を使えない場合は、DOMPurifyなどのライブラリによるサニタイズや、CSP（Content Security Policy）を組み合わせて対策します。
