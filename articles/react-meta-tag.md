---
title: '[React] metaタグを理解する' # 記事のタイトル
emoji: '🧜‍♀️' # アイキャッチとして使われる絵文字（1文字だけ）
type: 'tech' # tech: 技術記事 / idea: アイデア記事
topics: ['react', 'typescript', 'nodejs', '初心者向け'] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開設定（falseにすると下書き）
---

## はじめに
**metaタグとは**
>meta（メタ）タグとは、検索エンジンやブラウザなどに対してWebページの情報を伝えるHTMLタグです。
文字コードの指定や、検索エンジンへのインデックス（検索結果に表示される）の可否など、メタデータと呼ばれる情報を伝えるのがメインの役割となります。
ページタイトル（title）やページの説明文（description）など以外は、ユーザーの目入ることはほとんどありませんが、間接的にSEO効果に影響してくるのでとても重要な要素です。

@[card](https://and-ha.com/coding/meta/)

## 1. 設定するmetaタグを確認する

### 1. 文字コード（charset属性）
````html
<meta charset="utf-8">
````
HTML文書の文字エンコーディングを指定するための記述です。
文字コードの設定をしていないと、
英語版のブラウザから日本語サイトにアクセスすると文字化けが起きる可能性があります。
基本的には`UTF-8`で設定する

***
### 2. robotsタグ
````html
<meta name="robots" content="noindex, nofollow">
````
検索エンジンへのインデックスを避けるための記述です。
上記の記述をすることにより、検索結果として表示されなくなります。
低品質なページなどをインデックスさせないことで、SEOのマイナス評価を避けることにも繋がるので必要に応じて設定しましょう。
:::message
[注意] 逆にインデックスさせたいページには間違って設定しない
:::

***
### 3. format-detectionタグ
````html
<meta name="format-detection" content="telephone=no, email=no, address=no">
````
主にスマートフォンなどが電話番号を勝手にリンク設定することを避けるための記述です。
スマートフォンのブラウザなどでは、ページ内に電話番号のような記述を見つけると勝手にリンクにしてしまう場合があります。誤ってユーザーが架電してしまわないよう設定しておきましょう。

***
### 4. viewportタグ
````html
<meta name="viewport" content="width=device-width,initial-scale=1,minimum-scale=1.0">
````
ページの表示領域を指定するための記述です。
ブラウザがページを表示するときにどの大きさで表示するかを指定することができます。
この設定をしていないと、PCとスマートフォン、タブレットなど各デバイスごとに画面幅が違うブラウザで見た時に、ページが小さく表示されてしまったり、はみ出してしまったりする場合があります。
レスポンシブ対応が当たり前になってきている中では必須の設定です。

- **width=device-width**: <br>ページの表示幅をデバイスの幅に合わせるという意味です。これにより、ページがデバイスの幅に応じて自動的に適切なサイズに調整されます。
- **initial-scale=1**: <br>ページの初期表示時の拡大率を1倍に設定します。つまり、ページをデフォルトのサイズで開いた際に、拡大や縮小が行われないようにします。
- **minimum-scale=1.0**: <br>ページの最小拡大率を1.0倍に設定します。これにより、ユーザーがページを縮小することを制限し、コンテンツが読みやすい状態を維持します。

***
### 5. titleタグ
````html
<title>ページタイトル</title>
````
ページのタイトルを指定する記述です。

***
### 6. favicon（ファビコン）
````html
<link rel="icon" type="image/png" href="./assets/favicon.png">
````
ファビコンを設定するための記述です。
ファビコンとは、ブラウザのタブやブックマークバーなどに表示されるアイコンのことです。
サイトのシンボルマーク・イメージ付けとして大事なアイコンでもあるので、できるだけ設定しておくことをおすすめします。

***
### 7. apple-touch-icon
````html
<link rel="apple-touch-icon" href="./assets/apple-touch-icon.png" sizes="180x180">
````
スマートフォンでホーム画面に追加したときや、ショートカットを作成した場合に表示されるアイコンを設定するための記述です。

## 2. 実際にmetaタグを設定する
`src/frontend/public/index.html`を編集します。
先ほどの解説した必要な設定を`<head>`に追加します

追加した結果が下記ファイルになります。
````html:index.html
<!-- 省略 -->
  <head>
    <meta charset="utf-8" />
    <meta name="robots" content="noindex, nofollow">
    <meta name="format-detection" content="telephone=no, email=no, address=no">
    <meta name="viewport" content="width=device-width,initial-scale=1,minimum-scale=1.0">
    <title>Sandbooks</title>
    <link rel="icon" type="image/png" href="./assets/favicon.png">
    <link rel="apple-touch-icon" href="./assets/apple-touch-icon.png" sizes="180x180">
  </head>
<!-- 省略 -->
````

## 3. 他の設定すべき要素も設定する
`<head>`以外にも設定する箇所があります。
1. `lang="en"`を`lang="ja"`に変更する
2. チュートリアルのコメントを削除する
````html:index.html
<!DOCTYPE html>
<html lang="ja">
  <head>
    <meta charset="utf-8" />
    <meta name="robots" content="noindex, nofollow">
    <meta name="format-detection" content="telephone=no, email=no, address=no">
    <meta name="viewport" content="width=device-width,initial-scale=1,minimum-scale=1.0">
    <title>Sandbooks</title>
    <link rel="icon" type="image/png" href="./assets/favicon.png">
    <link rel="apple-touch-icon" href="./assets/apple-touch-icon.png" sizes="180x180">
  </head>
  <body>
    <!-- ユーザーがJavaScriptを無効にしている場合に代替のコンテンツやメッセージを表示するために使用されるHTMLタグです。 -->
    <noscript>You need to enable JavaScript to run this app.</noscript>
    <div id="root"></div>
  </body>
</html>
````