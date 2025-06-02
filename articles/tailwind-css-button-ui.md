---
title: '[Tailwind CSS] ボタンUI' # 記事のタイトル
emoji: '🍃' # アイキャッチとして使われる絵文字（1文字だけ）
type: 'tech' # tech: 技術記事 / idea: アイデア
topics: ['css', 'tailwindcss', '初心者向け'] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開設定（falseにすると下書き）
---

## はじめに

この記事では、**Tailwind CSS でよくあるボタンデザイン**を作成しました。

下記サイトを活用して表示しております。
@[card](https://play.tailwindcss.com/)

:::details 参考資料
@[card](https://gihyo.jp/book/2024/978-4-297-13943-8)
:::

## 1. 「登録」のボタン

![button01](/images/articles/tailwind-css-cheat-design/button01.png)

```html
<button class="py-1 px-5 bg-sky-500 rounded-2xl text-white font-black">
  登録
</button>
```

:::message
**工夫ポイント**

1. プライマリーカラーなど色を使用することで、この後にイベントが発生することをイメージさせる
2. 文字を白色にすることで、見やすさとテキストではないことを強調させる
3. ボタンの角を丸くすることで、ボタンらしさを強調させる
   :::

## 2. 「削除」のボタン

![button02](/images/articles/tailwind-css-cheat-design/button02.png)

```html
<button class="py-1 px-5 bg-red-500 rounded-2xl text-white font-black">
  削除
</button>
```

:::message
**工夫ポイント**

1. 警告カラーを使用することで、この後に重大なイベントが発生することをイメージさせる
2. 文字を白色にすることで、見やすさとテキストではないことを強調させる
3. ボタンの角を丸くすることで、ボタンらしさを強調させる
   :::

## 3. 「戻る」のボタン

![button03](/images/articles/tailwind-css-cheat-design/button03.png)

```html
<button class="py-1 px-5 bg-gray-100/50 border rounded-2xl">戻る</button>
```

:::message
**工夫ポイント**

1. これまでのボタンと異なりグレーを使用することで、この後に無害なイベントが発生することをイメージさせる
2. ボタンの角を丸くすることで、ボタンらしさを強調させる
   :::

## YouTube のご案内

ポモドーロタイマー（25 分勉強＋ 5 分休憩）を活用した作業・勉強配信を行っています。
集中したいときや、誰かと一緒に頑張りたいときに、ぜひご活用ください。

ご興味のある方は、ぜひお気軽に遊びに来てください！
「Zenn から来ました!!」とコメントを貰えると泣いて喜びます 🤣

@[card](https://www.youtube.com/@aew2sbee)
