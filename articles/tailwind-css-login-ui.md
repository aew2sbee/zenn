---
title: '[Tailwind CSS] ログイン画面UI' # 記事のタイトル
emoji: '🍃' # アイキャッチとして使われる絵文字（1文字だけ）
type: 'tech' # tech: 技術記事 / idea: アイデア
topics: ['ui', 'css', 'tailwindcss', '初心者向け'] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開設定（falseにすると下書き）
---

## はじめに

この記事では、**ログイン画面 UI の実装方法**を解説します。
下記サイトを活用して表示しております。
@[card](https://play.tailwindcss.com/)

:::details 参考資料
@[card](https://gihyo.jp/book/2024/978-4-297-13943-8)
:::

## 1. ログイン情報入力前画面

![enter-before](/images/articles/tailwind-css-login-ui/enter-before.png)
_ボタンに hover していないとき_
![hover](/images/articles/tailwind-css-login-ui/hover.png)
_ボタンに hover したとき_
:::message
**工夫した点**

1. ボタン hover 時に色を少し濃くし押すことが出来ることをアピール
2. 入力欄の右上に入力項目を記載し何を入力すべきかを視覚的にアピール
3. 周りを薄いグレーにすることで中央の白い画面目立たせてを視覚的にアピール
   :::

```html
<div class="bg-gray-100 min-h-screen flex items-center justify-center">
  <div class="bg-white p-8 rounded-lg shadow-lg w-96">
    <h2 class="text-2xl font-bold mb-6 text-center text-gray-800">ログイン</h2>
    <form action="#" method="POST">
      <div class="mb-4">
        <label for="email" class="block text-sm font-medium text-gray-700"
          >メールアドレス</label
        >
        <input
          type="email"
          id="email"
          name="email"
          placeholder="your@example.com"
          required
          class="mt-1 w-full px-4 py-2 border border-gray-300 rounded-md shadow-sm"
        />
      </div>
      <div class="mb-6 relative">
        <label for="password" class="block text-sm font-medium text-gray-700"
          >パスワード</label
        >
        <div class="relative">
          <input
            type="password"
            id="password"
            name="password"
            placeholder="••••••••"
            required
            class="mt-1 w-full px-4 py-2 border border-gray-300 rounded-md shadow-sm"
          />
        </div>
      </div>
      <div class="flex items-center justify-between mb-4">
        <div class="flex items-center">
          <input
            type="checkbox"
            id="remember"
            name="remember"
            class="h-4 w-4 text-blue-600 border-gray-300 rounded"
          />
          <label for="remember" class="ml-2 block text-sm text-gray-900"
            >ログイン状態を保持</label
          >
        </div>
      </div>
      <button
        type="submit"
        class="w-full bg-sky-500 text-white py-2 px-4 rounded-md shadow hover:bg-sky-600"
      >
        ログイン
      </button>
    </form>
  </div>
</div>
```

## 2. ログイン失敗画面

![error-message](/images/articles/tailwind-css-login-ui/error-message.png)
:::message
**工夫した点**

1. エラーメッセージを警告色で出力し視覚的にアピール
2. メールアドレス/パスワードの入力欄を警告色で表現し誤り箇所を視覚的にアピール
   :::

```html
<div class="bg-gray-100 min-h-screen flex items-center justify-center">
  <div class="bg-white p-8 rounded-lg shadow-lg w-96">
    <h2 class="text-2xl font-bold mb-6 text-center text-gray-800">ログイン</h2>
    <div class="bg-red-100 text-red-700 p-4 rounded-md mb-4 text-sm">
      メールアドレスまたはパスワードが間違っています。
    </div>
    <form action="#" method="POST">
      <div class="mb-4">
        <label for="email" class="block text-sm font-medium text-gray-700"
          >メールアドレス</label
        >
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
        <label for="password" class="block text-sm font-medium text-gray-700"
          >パスワード</label
        >
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
          <input
            type="checkbox"
            id="remember"
            name="remember"
            class="h-4 w-4 text-blue-600 border-gray-300 rounded"
          />
          <label for="remember" class="ml-2 block text-sm text-gray-900"
            >ログイン状態を保持</label
          >
        </div>
      </div>
      <button
        type="submit"
        class="w-full bg-sky-500 text-white py-2 px-4 rounded-md shadow hover:bg-sky-600"
      >
        ログイン
      </button>
    </form>
  </div>
</div>
```

## YouTube のご案内

ポモドーロタイマー（25 分勉強＋ 5 分休憩）を活用した作業・勉強配信を行っています。
集中したいときや、誰かと一緒に頑張りたいときに、ぜひご活用ください。

ご興味のある方は、ぜひお気軽に遊びに来てください！

@[card](https://www.youtube.com/@aew2sbee)
