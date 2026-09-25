---
title: "[React] metaタグを理解する" # 記事のタイトル
emoji: "🧜‍♀️" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア記事
topics: ["react", "typescript", "nodejs", "html", "初心者向け"] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開:true / 非公開:false
---

## 🌱 はじめに

**meta タグとは**

meta タグは、検索エンジンやブラウザに対して、Web ページの情報（メタデータ）を伝える HTML タグです。
文字コードや、検索結果に表示させるかどうかなどを指定できます。
画面にはほとんど表示されませんが、SEO やスマートフォンでの見え方に関わる大切な要素です。

参考: [meta タグとは？（株式会社And Ha）](https://and-ha.com/coding/meta/)

@[card](https://developer.mozilla.org/ja/docs/Web/HTML/Element/meta)

## 🌱 1. 設定する meta タグを確認する

### 1. 文字コード（charset 属性）

```html
<meta charset="utf-8" />
```

HTML の文字コードを指定します。
指定がないとブラウザが文字コードを推測するため、推測が外れると文字化けする場合があります。
基本的には`UTF-8`を指定し、`<head>`の先頭に書きます。

---

### 2. robots タグ

```html
<meta name="robots" content="noindex, nofollow" />
```

検索エンジンに対して、ページを検索結果に表示しない（`noindex`）、ページ内のリンクをたどらない（`nofollow`）ように伝えます。
検証用のページなど、検索結果に出したくないページに設定します。

:::message alert
React（Create React App）のような SPA では、すべてのページが同じ`index.html`から配信されます。
`index.html`に`noindex`を書くとサイト全体が検索結果に表示されなくなるため、公開するサイトには設定しないでください。
:::

参考: [robots meta タグ（Google 検索セントラル）](https://developers.google.com/search/docs/crawling-indexing/robots-meta-tag?hl=ja)

---

### 3. format-detection タグ

```html
<meta name="format-detection" content="telephone=no, email=no, address=no" />
```

主に iOS の Safari で、電話番号・メールアドレス・住所のように見える文字列が自動でリンクになるのを防ぎます（Apple 独自の meta タグです）。

---

### 4. viewport タグ

```html
<meta name="viewport" content="width=device-width,initial-scale=1,minimum-scale=1.0" />
```

スマートフォンなどで、ページをどの幅・倍率で表示するかを指定します。
指定がないと、スマートフォンで PC 向けの幅のまま縮小して表示される場合があります。

- **width=device-width**: <br>表示幅をデバイスの画面幅に合わせます。
- **initial-scale=1**: <br>最初に表示するときの倍率を1倍にします（ユーザーはあとから拡大・縮小できます）。
- **minimum-scale=1.0**: <br>1倍より小さく縮小できないようにします（拡大は制限しません）。

:::message
文字を読むために拡大が必要な人もいるため、`maximum-scale`や`user-scalable=no`で拡大を禁止しないようにしましょう。
:::

参考: [ビューポートメタタグ（MDN）](https://developer.mozilla.org/ja/docs/Web/HTML/Viewport_meta_tag)

---

### 5. title タグ

```html
<title>ページタイトル</title>
```

ページのタイトルを指定します。ブラウザのタブや検索結果に表示されます。

---

### 6. favicon（ファビコン）

```html
<link rel="icon" type="image/png" href="%PUBLIC_URL%/assets/favicon.png" />
```

ブラウザのタブやブックマークに表示されるアイコンを指定します。

---

### 7. apple-touch-icon

```html
<link rel="apple-touch-icon" href="%PUBLIC_URL%/assets/apple-touch-icon.png" sizes="180x180" />
```

iPhone や iPad で、ホーム画面に追加したときに表示されるアイコンを指定します。
（Android では、`manifest.json`の`icons`が使われます）

:::message
Create React App では、アイコンなどのファイルを`public`フォルダに置き、`%PUBLIC_URL%`を使って指定します。
`./assets/favicon.png`のような相対パスにすると、`/users/1`のような深い URL で開いたときにアイコンが読み込めない場合があります。
:::

## 🌱 2. 実際に meta タグを設定する

Create React App で作成したプロジェクトの`src/frontend/public/index.html`を編集します。
先ほど解説した設定を`<head>`に追加します。

追加した結果が下記ファイルになります。

```html:index.html
<!-- 省略 -->
  <head>
    <meta charset="utf-8" />
    <meta name="robots" content="noindex, nofollow" />
    <meta name="format-detection" content="telephone=no, email=no, address=no" />
    <meta name="viewport" content="width=device-width,initial-scale=1,minimum-scale=1.0" />
    <title>Sandbooks</title>
    <link rel="icon" type="image/png" href="%PUBLIC_URL%/assets/favicon.png" />
    <link rel="apple-touch-icon" href="%PUBLIC_URL%/assets/apple-touch-icon.png" sizes="180x180" />
  </head>
<!-- 省略 -->
```

:::message
本記事の例では`noindex, nofollow`を設定しています。公開するサイトでは設定しないでください。
また、Create React App の初期ファイルにある`description`・`theme-color`・`manifest`の設定は削除しています。公開するサイトでは残すことをおすすめします。
:::

## 🌱 3. その他の箇所も修正する

`<head>`以外にも修正する箇所があります。

1. `lang="en"`を`lang="ja"`に変更する
2. チュートリアルのコメントを削除し、説明用のコメントに差し替える

```html:index.html
<!DOCTYPE html>
<html lang="ja">
  <head>
    <meta charset="utf-8" />
    <meta name="robots" content="noindex, nofollow" />
    <meta name="format-detection" content="telephone=no, email=no, address=no" />
    <meta name="viewport" content="width=device-width,initial-scale=1,minimum-scale=1.0" />
    <title>Sandbooks</title>
    <link rel="icon" type="image/png" href="%PUBLIC_URL%/assets/favicon.png" />
    <link rel="apple-touch-icon" href="%PUBLIC_URL%/assets/apple-touch-icon.png" sizes="180x180" />
  </head>
  <body>
    <!-- ユーザーがJavaScriptを無効にしている場合に代替のコンテンツやメッセージを表示するために使用されるHTMLタグです。 -->
    <noscript>You need to enable JavaScript to run this app.</noscript>
    <div id="root"></div>
  </body>
</html>
```
