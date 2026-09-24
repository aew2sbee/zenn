---
title: "[Tailwind CSS] ログイン画面UI" # 記事のタイトル
emoji: "🍃" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["ui", "css", "tailwindcss", "初心者向け"] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開:true / 非公開:false
---

## 🌱 はじめに

この記事では、**ログイン画面 UI の実装方法**を解説します。
画面は、下記の Tailwind Play で表示を確認しています。各章の HTML を Tailwind Play の左側の HTML 欄に貼り付けると、右側に表示されます。

@[card](https://play.tailwindcss.com/)

:::message
現在の Tailwind Play は Tailwind CSS v4 で動いています。v4 では影の大きさの段階が変わったため、影がスクリーンショットより一段小さく表示されます。画像と同じ見た目にしたい場合は、`shadow-sm`を`shadow-xs`に、`shadow`を`shadow-sm`に置き換えてください。
:::

:::details 参考資料
@[card](https://gihyo.jp/book/2024/978-4-297-13943-8)
:::

## 🌱 1. ログイン情報入力前画面

![enter-before](/images/articles/tailwind-css-login-ui/enter-before.png)
_ボタンに hover していないとき_
![hover](/images/articles/tailwind-css-login-ui/hover.png)
_ボタンに hover したとき_
:::message
**工夫した点**

1. ボタンに hover したときに色を少し濃くし、押せることをアピール
2. 入力欄の左上に入力項目を記載し、何を入力すべきかを視覚的にアピール
3. 周りを薄いグレーにして、中央の白いフォームを目立たせる
:::

```html
<div class="bg-gray-100 min-h-screen flex items-center justify-center">
  <div class="bg-white p-8 rounded-lg shadow-lg w-96">
    <h2 class="text-2xl font-bold mb-6 text-center text-gray-800">ログイン</h2>
    <form action="#" method="POST">
      <div class="mb-4">
        <label for="email" class="block text-sm font-medium text-gray-700">メールアドレス</label>
        <input
          type="email"
          id="email"
          name="email"
          placeholder="your@example.com"
          required
          class="mt-1 w-full px-4 py-2 border border-gray-300 rounded-md shadow-sm"
        />
      </div>
      <div class="mb-6">
        <label for="password" class="block text-sm font-medium text-gray-700">パスワード</label>
        <input
          type="password"
          id="password"
          name="password"
          placeholder="••••••••"
          required
          class="mt-1 w-full px-4 py-2 border border-gray-300 rounded-md shadow-sm"
        />
      </div>
      <div class="flex items-center justify-between mb-4">
        <div class="flex items-center">
          <input type="checkbox" id="remember" name="remember" class="h-4 w-4 accent-sky-500" />
          <label for="remember" class="ml-2 block text-sm text-gray-900">ログイン状態を保持</label>
        </div>
      </div>
      <button type="submit" class="w-full bg-sky-500 text-white py-2 px-4 rounded-md shadow hover:bg-sky-600">
        ログイン
      </button>
    </form>
  </div>
</div>
```

## 🌱 2. ログイン失敗画面

![error-message](/images/articles/tailwind-css-login-ui/error-message.png)
:::message
**工夫した点**

1. エラーメッセージを警告色で表示し、視覚的にアピール
2. メールアドレス/パスワードの入力欄を警告色で表現し、誤り箇所を視覚的にアピール
:::

※ エラー時の見た目を確認するための静的な HTML です。ログインの判定処理は含みません。

```html
<div class="bg-gray-100 min-h-screen flex items-center justify-center">
  <div class="bg-white p-8 rounded-lg shadow-lg w-96">
    <h2 class="text-2xl font-bold mb-6 text-center text-gray-800">ログイン</h2>
    <div class="bg-red-100 text-red-700 p-4 rounded-md mb-4 text-sm">
      メールアドレスまたはパスワードが間違っています。
    </div>
    <form action="#" method="POST">
      <div class="mb-4">
        <label for="email" class="block text-sm font-medium text-gray-700">メールアドレス</label>
        <input
          type="email"
          id="email"
          name="email"
          placeholder="your@example.com"
          required
          class="mt-1 w-full px-4 py-2 border border-red-500 rounded-md shadow-sm"
        />
      </div>
      <div class="mb-6">
        <label for="password" class="block text-sm font-medium text-gray-700">パスワード</label>
        <input
          type="password"
          id="password"
          name="password"
          placeholder="••••••••"
          required
          class="mt-1 w-full px-4 py-2 border border-red-500 rounded-md shadow-sm"
        />
      </div>
      <div class="flex items-center justify-between mb-4">
        <div class="flex items-center">
          <input type="checkbox" id="remember" name="remember" class="h-4 w-4 accent-sky-500" />
          <label for="remember" class="ml-2 block text-sm text-gray-900">ログイン状態を保持</label>
        </div>
      </div>
      <button type="submit" class="w-full bg-sky-500 text-white py-2 px-4 rounded-md shadow hover:bg-sky-600">
        ログイン
      </button>
    </form>
  </div>
</div>
```
