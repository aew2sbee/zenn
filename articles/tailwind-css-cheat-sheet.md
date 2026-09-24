---
title: "[Tailwind CSS] 私なりのチートシート" # 記事のタイトル
emoji: "🍃" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["css", "tailwindcss", "初心者向け"] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開:true / 非公開:false
---

## 🌱 はじめに

この記事では、**基本的な Tailwind CSS**を解説します。

@[card](https://gihyo.jp/book/2024/978-4-297-13943-8)

## 🌱 前提条件

1. 下記サイトで表示を確認しています。

   @[card](https://play.tailwindcss.com/)

2. 見やすいように`font-size`を`200%`にしています。

   ```css
   html {
     font-size: 200%;
   }
   ```

3. 見やすいように下記の`div`で囲っています。

   ```html
   <div class="flex flex-col justify-center min-h-screen mx-10">
     <!-- 対象のhtmlを記載 -->
   </div>
   ```

4. 見やすいように**背景色(`bg-cyan-X00`)** を設定している場合があります。

5. 記事中の px の値は、ルートの文字サイズが 16px（`1rem = 16px`）の場合の値です。


:::message
本記事は Tailwind CSS v3 の仕様で書いています。v4 では、色の値が rgb から oklch に変わったほか、`shadow-sm`→`shadow-xs`、`shadow`→`shadow-sm`、`rounded`→`rounded-sm`のように一部のクラス名が変わりました。
:::

## 🌱 背景

### 1. 背景色を設定する

```html
<p class="">backgroundColor未設定</p>
<p class="bg-cyan-500">backgroundColorをrgb(6, 182, 212)に設定</p>
```

![bk](/images/articles/tailwind-css-cheat-sheet/bk.png)

### 2. 背景色の不透明度を設定する

```html
<p class="bg-cyan-500">backgroundColorをrgb(6, 182, 212)に設定</p>
<p class="bg-cyan-500/50">backgroundColorをrgb(6, 182, 212, 0.5)に設定</p>
<p class="bg-cyan-500/30">backgroundColorをrgb(6, 182, 212, 0.3)に設定</p>
```

![bk-opacity](/images/articles/tailwind-css-cheat-sheet/bk-opacity.png)

## 🌱 文字

### 1. 文字色を設定する

```html
<p class="">textColor未設定</p>
<p class="text-red-500">textColorをrgb(239, 68, 68)に設定</p>
```

![text-color](/images/articles/tailwind-css-cheat-sheet/text-color.png)

### 2. 文字色の不透明度を設定する

```html
<p class="text-red-500">textColorをrgb(239, 68, 68)に設定</p>
<p class="text-red-500/50">textColorをrgb(239, 68, 68, 0.5)に設定</p>
<p class="text-red-500/30">textColorをrgb(239, 68, 68, 0.3)に設定</p>
```

![text-color-opacity](/images/articles/tailwind-css-cheat-sheet/text-color-opacity.png)

### 3. 文字サイズを設定する

```html
<p class="bg-cyan-100 text-xs">font-size: 0.75rem; line-height: 1rem;</p>
<p class="bg-cyan-200 text-base">font-size: 1rem; line-height: 1.5rem;</p>
<p class="bg-cyan-300 text-lg">font-size: 1.125rem; line-height: 1.75rem;</p>
<p class="bg-cyan-400 text-xl">font-size: 1.25rem; line-height: 1.75rem;</p>
<p class="bg-cyan-500 text-2xl">font-size: 1.5rem; line-height: 2rem;</p>
```

![text-size](/images/articles/tailwind-css-cheat-sheet/text-size.png)

### 4. 行の高さを設定する

```html
<p class="bg-cyan-100 leading-3">line-height: .75rem</p>
<p class="bg-cyan-200 leading-4">line-height: 1rem</p>
<p class="bg-cyan-300 leading-5">line-height: 1.25rem</p>
<p class="bg-cyan-400 leading-6">line-height: 1.5rem</p>
<p class="bg-cyan-500 leading-7">line-height: 1.75rem</p>
<p class="bg-cyan-600 leading-8">line-height: 2rem</p>
```

![text-leading](/images/articles/tailwind-css-cheat-sheet/text-leading.png)

### 5. 文字の太さを設定する

```html
<p class="font-thin">font-weight: 100</p>
<p class="font-light">font-weight: 300</p>
<p class="font-normal">font-weight: 400</p>
<p class="font-bold">font-weight: 700</p>
<p class="font-black">font-weight: 900</p>
```

![font-weight](/images/articles/tailwind-css-cheat-sheet/font-weight.png)

:::message
画像の`font-weight: 700`の行は、誤ったクラス名（`font-blod`）で表示したため、太字になっていません。
:::

### 6. 文字の配置(左右)を設定する

```html
<p class="text-left">text-align: left</p>
<p class="text-center">text-align: center</p>
<p class="text-right">text-align: right</p>
<p class="text-justify">text-align: justify</p>
<p class="text-start">text-align: start</p>
<p class="text-end">text-align: end</p>
```

![text-align](/images/articles/tailwind-css-cheat-sheet/text-align.png)

:::details [Q] text-align: left or justify or start の違いとは？

| プロパティ | 揃え方 | 書字方向依存 | 用途 |
| ---- | ---- | ---- | ---- |
| text-align: left | 左揃え | 依存しない | 主に左から右書字方向の言語向け |
| text-align: justify | 両端揃え（最終行は開始側に揃う） | 最終行は依存する | 美観が求められる文章配置向け |
| text-align: start | 書字方向の開始側揃え | 依存する | 国際化対応、動的言語切替に適している |

:::

### 7. 数字の幅をそろえる

:::message
数字の字幅をそろえて（等幅数字で）表示する設定です。フォントが対応している場合にだけ効きます。
:::

```html
<p class="tabular-nums">1234567890</p>
<p class="tabular-nums">0909090909</p>
```

![tabular-nums](/images/articles/tailwind-css-cheat-sheet/tabular-nums.png)

### 8. 省略と改行を設定する

```html
<div class="mb-3">
  <p class="font-black">1行目まで表示</p>
  <p class="line-clamp-1 pl-3">ああああああああああああああああああああああああああああああああああああああああ</p>
</div>
<div class="mb-3">
  <p class="font-black">2行目まで表示</p>
  <p class="line-clamp-2 pl-3">
    あああああああああああああああああああああああああああああああああああああああ
    いいいいいいいいいいいいいいいいいいいいいいいいいいいいいいいいいいいいいいいい
  </p>
</div>
```

![line-clamp](/images/articles/tailwind-css-cheat-sheet/line-clamp.png)

## 🌱 margin/padding

### 1. 四方にスペーシングを設定する

```html
<div class="bg-cyan-200 m-6">
  <div class="h-20 w-20">margin: 24px</div>
</div>
<div class="bg-rose-200 p-6">
  <div class="h-20 w-20">padding: 24px</div>
</div>
```

![m-p](/images/articles/tailwind-css-cheat-sheet/m-p.png)

### 2. 上側にスペーシングを設定する

```html
<div class="bg-cyan-200 mt-6">
  <div class="h-20 w-20">margin: 24px</div>
</div>
<div class="bg-rose-200 pt-6">
  <div class="h-20 w-20">padding: 24px</div>
</div>
```

![m-p-t](/images/articles/tailwind-css-cheat-sheet/m-p-t.png)

### 3. 右側にスペーシングを設定する

```html
<div class="bg-cyan-200 mr-6">
  <div class="h-20 w-20">margin: 24px</div>
</div>
<div class="bg-rose-200 pr-6">
  <div class="h-20 w-20">padding: 24px</div>
</div>
```

![m-p-r](/images/articles/tailwind-css-cheat-sheet/m-p-r.png)

### 4. 下側にスペーシングを設定する

```html
<div class="bg-cyan-200 mb-6">
  <div class="h-20 w-20">margin: 24px</div>
</div>
<div class="bg-rose-200 pb-6">
  <div class="h-20 w-20">padding: 24px</div>
</div>
```

![m-p-b](/images/articles/tailwind-css-cheat-sheet/m-p-b.png)

### 5. 左側にスペーシングを設定する

```html
<div class="bg-cyan-200 ml-6">
  <div class="h-20 w-20">margin: 24px</div>
</div>
<div class="bg-rose-200 pl-6">
  <div class="h-20 w-20">padding: 24px</div>
</div>
```

![m-p-l](/images/articles/tailwind-css-cheat-sheet/m-p-l.png)

### 6. 左右側にスペーシングを設定する

```html
<div class="bg-cyan-200 mx-6">
  <div class="h-20 w-20">margin: 24px</div>
</div>
<div class="bg-rose-200 px-6">
  <div class="h-20 w-20">padding: 24px</div>
</div>
```

![m-p-x](/images/articles/tailwind-css-cheat-sheet/m-p-x.png)

### 7. 上下側にスペーシングを設定する

```html
<div class="bg-cyan-200 my-6">
  <div class="h-20 w-20">margin: 24px</div>
</div>
<div class="bg-rose-200 py-6">
  <div class="h-20 w-20">padding: 24px</div>
</div>
```

![m-p-y](/images/articles/tailwind-css-cheat-sheet/m-p-y.png)

### 8. 負の値でマージンを設定する

```html
<div class="bg-cyan-200 -m-6">
  <div class="h-20 w-20">margin: -24px</div>
</div>
```

![-m-p](/images/articles/tailwind-css-cheat-sheet/-m-p.png)

:::message
負の値を指定できるのは margin だけです。padding に負の値はありません（`-p-6`というクラスは存在しません）。画像の下段は、存在しない`-p-6`を指定して表示したもので、padding は設定されていません。
:::

### 9. 左右側にスペーシングを間隔で設定する

```html
<div class="flex mx-10 bg-gray-50 space-x-4">
  <div class="h-20 w-20 bg-cyan-200">1</div>
  <div class="h-20 w-20 bg-cyan-200">2</div>
  <div class="h-20 w-20 bg-cyan-200">3</div>
</div>
```

![space-x](/images/articles/tailwind-css-cheat-sheet/space-x.png)

### 10. 上下側にスペーシングを間隔で設定する

```html
<div class="flex flex-col justify-center items-start min-h-screen mx-10 bg-gray-50 space-y-4">
  <div class="h-20 w-20 bg-cyan-200">1</div>
  <div class="h-20 w-20 bg-cyan-200">2</div>
  <div class="h-20 w-20 bg-cyan-200">3</div>
</div>
```

![space-y](/images/articles/tailwind-css-cheat-sheet/space-y.png)

### 11. box-sizing を設定する

```html
<div class="bg-cyan-100 box-border h-32 w-32 pr-4 border-r-4 border-sky-300">
  <p class="bg-cyan-100">横幅=128px（padding と border を含む）</p>
</div>
<div class="bg-cyan-100 box-content h-32 w-32 pr-4 border-r-4 border-sky-300">
  <p class="bg-cyan-300">横幅=148px（128px + padding 16px + border 4px）</p>
</div>
```

![box-sizing](/images/articles/tailwind-css-cheat-sheet/box-sizing.png)

:::message
画像内の文字は修正前のもの（どちらも「横幅=128px」）です。下段の`box-content`は、width の外側に padding と border が加わるため、全体の横幅は 148px になります。
:::

## 🌱 ボーダー

### 1. 境界線を設定する

```html
<div class="h-20 w-20 bg-cyan-200">境界線なし</div>
<div class="h-20 w-20 bg-rose-200 border border-indigo-500">境界線あり<br />1px solid</div>
```

![border](/images/articles/tailwind-css-cheat-sheet/border.png)

### 2. 境界線の太さを設定する

```html
<div class="h-20 w-20 bg-cyan-200 border-indigo-500 border">1px</div>
<div class="h-20 w-20 bg-rose-200 border-indigo-500 border-4">4px</div>
```

![border-weight](/images/articles/tailwind-css-cheat-sheet/border-weight.png)

### 3. 境界線の位置を設定する

```html
<div class="h-10 w-10 bg-cyan-200 border-indigo-500 border-t-4">上</div>
<div class="h-10 w-10 bg-cyan-200 border-indigo-500 border-r-4">右</div>
<div class="h-10 w-10 bg-cyan-200 border-indigo-500 border-b-4">下</div>
<div class="h-10 w-10 bg-cyan-200 border-indigo-500 border-l-4">左</div>
<div class="h-10 w-10 bg-cyan-200 border-indigo-500 border-x-4">左右</div>
<div class="h-10 w-10 bg-cyan-200 border-indigo-500 border-y-4">上下</div>
```

![border-position](/images/articles/tailwind-css-cheat-sheet/border-position.png)

### 4. 境界線のスタイルを設定する

```html
<div class="h-10 w-20 bg-cyan-200 border-indigo-500 border-4 border-dashed">破線</div>
<div class="h-10 w-20 bg-cyan-200 border-indigo-500 border-4 border-dotted">ドット</div>
<div class="h-10 w-20 bg-cyan-200 border-indigo-500 border-4 border-double">二重線</div>
<div class="h-10 w-20 bg-cyan-200 border-indigo-500 border-4 border-hidden">非表示</div>
```

![border-style](/images/articles/tailwind-css-cheat-sheet/border-style.png)

## 🌱 横幅を設定する

### 1. 横幅を割合で設定する

```html
<div class="flex mt-2 mx-10 bg-gray-50">
  <div class="h-20 bg-cyan-200 w-2/5">40%</div>
  <div class="h-20 bg-rose-200 w-3/5">60%</div>
</div>
```

![w-percent](/images/articles/tailwind-css-cheat-sheet/w-percent.png)

### 2. w-full と w-auto で横幅を設定する

```html
<div class="flex mt-2 mx-10 bg-gray-50">
  <div class="h-20 bg-cyan-200 w-full">w-full: 親要素いっぱいに広がる</div>
  <div class="h-20 bg-rose-200 w-auto">w-auto: 内容やコンテンツのサイズに応じて広がる</div>
</div>
```

![w-auto-fill](/images/articles/tailwind-css-cheat-sheet/w-auto-fill.png)

### 3. 横幅の最大値を設定する

```html
<div class="p-4">
  <div class="bg-sky-300 max-w-xs">320px</div>
  <div class="bg-sky-300 max-w-sm">384px</div>
  <div class="bg-sky-300 max-w-md">448px</div>
  <div class="bg-sky-300 max-w-lg">512px</div>
  <div class="bg-sky-300 max-w-xl">576px</div>
  <div class="bg-sky-300 max-w-2xl">672px</div>
  <div class="bg-sky-300 max-w-3xl">768px</div>
  <div class="bg-sky-300 max-w-4xl">896px</div>
  <div class="bg-sky-300 max-w-5xl">1024px</div>
  <div class="bg-sky-300 max-w-6xl">1152px</div>
  <div class="bg-sky-300 max-w-7xl">1280px</div>
</div>
```

![max-w](/images/articles/tailwind-css-cheat-sheet/max-w.png)

## 🌱 縦幅を設定する

:::message
`h-1/2`や`h-full`のような割合の指定は、親要素に高さが指定されていないと効きません。
:::

### 1. 縦幅を割合で設定する

```html
<div class="flex flex-col h-40 mt-2 mx-10 bg-gray-50">
  <div class="bg-cyan-200 h-2/5">40%</div>
  <div class="bg-rose-200 h-3/5">60%</div>
</div>
```

### 2. h-full と h-auto で縦幅を設定する

```html
<div class="flex h-40 mt-2 mx-10 bg-gray-50">
  <div class="w-1/2 bg-cyan-200 h-full">h-full: 親要素いっぱいに広がる</div>
  <div class="w-1/2 bg-rose-200 h-auto">h-auto: 内容やコンテンツのサイズに応じて広がる</div>
</div>
```

## 🌱 角を設定する

### 1. 角の丸みを設定する

```html
<div class="w-20 bg-cyan-100 rounded">4px</div>
<div class="w-20 bg-cyan-200 rounded-md">6px</div>
<div class="w-20 bg-cyan-300 rounded-lg">8px</div>
<div class="w-20 bg-cyan-400 rounded-2xl">16px</div>
```

![rounded](/images/articles/tailwind-css-cheat-sheet/rounded.png)

:::message
画像内の「12px」は誤りで、`rounded-2xl`は 16px です（12px は`rounded-xl`）。
:::

## 🌱 レスポンシブ対応

### 1. 横幅に応じて非表示にする

```html
<div class="bg-sky-100 sm:hidden">640px以上の場合に非表示</div>
<div class="bg-sky-200 md:hidden">768px以上の場合に非表示</div>
<div class="bg-sky-300 lg:hidden">1024px以上の場合に非表示</div>
<div class="bg-sky-400 xl:hidden">1280px以上の場合に非表示</div>
<div class="bg-sky-500 2xl:hidden">1536px以上の場合に非表示</div>
```

:::message
Tailwind CSS はモバイルファーストのため、`sm:hidden`は「640px 以上で非表示（640px 未満では表示）」になります。画像内の文字は修正前の「〜未満の場合のみ非表示」のままですが、表示結果は上記のとおりです。
:::

![620px](/images/articles/tailwind-css-cheat-sheet/620px.png)
*横幅:620px*

![720px](/images/articles/tailwind-css-cheat-sheet/720px.png)
*横幅:720px*

![1020px](/images/articles/tailwind-css-cheat-sheet/1020px.png)
*横幅:1020px*

![1120px](/images/articles/tailwind-css-cheat-sheet/1120px.png)
*横幅:1120px*

![1520px](/images/articles/tailwind-css-cheat-sheet/1520px.png)
*横幅:1520px*

## 🌱 フレックス

### 1. 横並びにする

```html
<!-- 横並びしない -->
<div class="p-4">
  <div class="bg-sky-100">テキストが入ります</div>
  <div class="bg-sky-200">テキストが入ります</div>
  <div class="bg-sky-300">テキストが入ります</div>
  <div class="bg-sky-400">テキストが入ります</div>
  <div class="bg-sky-500">テキストが入ります</div>
</div>

<!-- 横並びする -->
<div class="p-4 flex flex-row">
  <div class="bg-sky-100">テキストが入ります</div>
  <div class="bg-sky-200">テキストが入ります</div>
  <div class="bg-sky-300">テキストが入ります</div>
  <div class="bg-sky-400">テキストが入ります</div>
  <div class="bg-sky-500">テキストが入ります</div>
</div>
```

![flex-row](/images/articles/tailwind-css-cheat-sheet/flex-row.png)

### 2. 逆向きに横並びにする

```html
<!-- 横並びしない -->
<div class="p-4">
  <div class="bg-sky-100">テキストが入ります</div>
  <div class="bg-sky-200">テキストが入ります</div>
  <div class="bg-sky-300">テキストが入ります</div>
  <div class="bg-sky-400">テキストが入ります</div>
  <div class="bg-sky-500">テキストが入ります</div>
</div>
<!-- 横並びする -->
<div class="p-4 flex flex-row-reverse">
  <div class="bg-sky-100">テキストが入ります</div>
  <div class="bg-sky-200">テキストが入ります</div>
  <div class="bg-sky-300">テキストが入ります</div>
  <div class="bg-sky-400">テキストが入ります</div>
  <div class="bg-sky-500">テキストが入ります</div>
</div>
```

![flex-row-reverse](/images/articles/tailwind-css-cheat-sheet/flex-row-reverse.png)

### 3. 縦並びにする

```html
<!-- 縦並びしない -->
<div class="p-4">
  <div class="bg-sky-100">テキストが入ります</div>
  <div class="bg-sky-200">テキストが入ります</div>
  <div class="bg-sky-300">テキストが入ります</div>
  <div class="bg-sky-400">テキストが入ります</div>
  <div class="bg-sky-500">テキストが入ります</div>
</div>
<!-- 縦並びする -->
<div class="p-4 flex flex-col">
  <div class="bg-sky-100">テキストが入ります</div>
  <div class="bg-sky-200">テキストが入ります</div>
  <div class="bg-sky-300">テキストが入ります</div>
  <div class="bg-sky-400">テキストが入ります</div>
  <div class="bg-sky-500">テキストが入ります</div>
</div>
```

![flex-col](/images/articles/tailwind-css-cheat-sheet/flex-col.png)

### 4. 逆向きに縦並びにする

```html
<!-- 縦並びしない -->
<div class="p-4">
  <div class="bg-sky-100">テキストが入ります</div>
  <div class="bg-sky-200">テキストが入ります</div>
  <div class="bg-sky-300">テキストが入ります</div>
  <div class="bg-sky-400">テキストが入ります</div>
  <div class="bg-sky-500">テキストが入ります</div>
</div>
<!-- 縦並びする -->
<div class="p-4 flex flex-col-reverse">
  <div class="bg-sky-100">テキストが入ります</div>
  <div class="bg-sky-200">テキストが入ります</div>
  <div class="bg-sky-300">テキストが入ります</div>
  <div class="bg-sky-400">テキストが入ります</div>
  <div class="bg-sky-500">テキストが入ります</div>
</div>
```

![flex-col-reverse](/images/articles/tailwind-css-cheat-sheet/flex-col-reverse.png)

### 5. 折り返す

```html
<!-- 折り返ししない -->
<div class="p-4 flex flex-row">
  <div class="bg-sky-100">テキストが入ります</div>
  <div class="bg-sky-200">テキストが入ります</div>
  <div class="bg-sky-300">テキストが入ります</div>
  <div class="bg-sky-400">テキストが入ります</div>
  <div class="bg-sky-500">テキストが入ります</div>
  <div class="bg-sky-600">テキストが入ります</div>
</div>
<!-- 折り返しする -->
<div class="p-4 flex flex-row flex-wrap">
  <div class="bg-sky-100">テキストが入ります</div>
  <div class="bg-sky-200">テキストが入ります</div>
  <div class="bg-sky-300">テキストが入ります</div>
  <div class="bg-sky-400">テキストが入ります</div>
  <div class="bg-sky-500">テキストが入ります</div>
  <div class="bg-sky-600">テキストが入ります</div>
</div>
```

![flex-wrap](/images/articles/tailwind-css-cheat-sheet/flex-wrap.png)

### 6. 逆向きに折り返す

```html
<!-- 折り返ししない -->
<div class="p-4 flex flex-row flex-wrap">
  <div class="bg-sky-100">テキストが入ります</div>
  <div class="bg-sky-200">テキストが入ります</div>
  <div class="bg-sky-300">テキストが入ります</div>
  <div class="bg-sky-400">テキストが入ります</div>
  <div class="bg-sky-500">テキストが入ります</div>
  <div class="bg-sky-600">テキストが入ります</div>
</div>
<!-- 折り返しする -->
<div class="p-4 flex flex-row flex-wrap-reverse">
  <div class="bg-sky-100">テキストが入ります</div>
  <div class="bg-sky-200">テキストが入ります</div>
  <div class="bg-sky-300">テキストが入ります</div>
  <div class="bg-sky-400">テキストが入ります</div>
  <div class="bg-sky-500">テキストが入ります</div>
  <div class="bg-sky-600">テキストが入ります</div>
</div>
```

![flex-wrap-reverse](/images/articles/tailwind-css-cheat-sheet/flex-wrap-reverse.png)

### 7. 子要素の横幅を割合で設定する

```html
<div class="p-4 flex flex-row flex-wrap">
  <div class="bg-sky-200 basis-1/2">basis-1/2</div>
  <div class="bg-sky-300 basis-1/2">basis-1/2</div>
  <div class="bg-sky-200 basis-1/4">basis-1/4</div>
  <div class="bg-sky-300 basis-3/4">basis-3/4</div>
  <div class="bg-sky-200 basis-2/5">basis-2/5</div>
  <div class="bg-sky-300 basis-3/5">basis-3/5</div>
  <div class="bg-sky-200 basis-2/6">basis-2/6</div>
  <div class="bg-sky-300 basis-4/6">basis-4/6</div>
  <div class="bg-sky-200 basis-4/12">basis-4/12</div>
  <div class="bg-sky-300 basis-8/12">basis-8/12</div>
</div>
```

![basis](/images/articles/tailwind-css-cheat-sheet/basis.png)

## 🌱 グリッド

### 1. n 列の要素に設定する

```html
<!-- 設定しない -->
<div class="p-4 grid">
  <div class="bg-sky-100">01</div>
  <div class="bg-sky-200">02</div>
  <div class="bg-sky-300">03</div>
  <div class="bg-sky-400">04</div>
  <div class="bg-sky-500">05</div>
  <div class="bg-sky-600">06</div>
</div>
<!-- 設定する -->
<div class="p-4 grid grid-cols-3">
  <div class="bg-sky-100">01</div>
  <div class="bg-sky-200">02</div>
  <div class="bg-sky-300">03</div>
  <div class="bg-sky-400">04</div>
  <div class="bg-sky-500">05</div>
  <div class="bg-sky-600">06</div>
</div>
```

![grid](/images/articles/tailwind-css-cheat-sheet/grid.png)

### 2. 横幅に応じて n 列の要素に設定する

```html
<div class="p-4 grid grid-cols-2 md:grid-cols-3">
  <div class="bg-sky-100">01</div>
  <div class="bg-sky-200">02</div>
  <div class="bg-sky-300">03</div>
  <div class="bg-sky-400">04</div>
  <div class="bg-sky-500">05</div>
  <div class="bg-sky-600">06</div>
</div>
```

![grid-720px](/images/articles/tailwind-css-cheat-sheet/grid-720px.png)
*横幅:720px*

![grid-1020px](/images/articles/tailwind-css-cheat-sheet/grid-1020px.png)
*横幅:1020px*

## 🌱 テーブル

### 1. 表にキャプションを設定する

```html
<table class="bg-sky-100">
  <caption class="caption-top">
    caption-top
  </caption>
  <caption class="caption-bottom">
    caption-bottom
  </caption>
  <thead>
    <tr>
      <th>Name</th>
      <th>Email</th>
      <th>Role</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>John Doe</td>
      <td>john.doe@example.com</td>
      <td>Admin</td>
    </tr>
  </tbody>
</table>
```

![caption](/images/articles/tailwind-css-cheat-sheet/caption.png)

:::message
HTML の仕様では、`<caption>`は1つの`<table>`に1つだけで、最初の子要素に置きます。ここでは比較のために2つ並べていますが、実際に使うときは1つにしてください。
:::

## 🌱 ブロックレイアウト

### 1. 自動で画面幅に応じて横幅を設定する

```html
<div class="container mx-auto px-4 bg-sky-100">
  <p>この要素は中央揃えで、画面幅に応じて調整されます。</p>
</div>
```

![container-620px](/images/articles/tailwind-css-cheat-sheet/container-620px.png)
*横幅:620px*

![container-720px](/images/articles/tailwind-css-cheat-sheet/container-720px.png)
*横幅:720px*

![container-1020px](/images/articles/tailwind-css-cheat-sheet/container-1020px.png)
*横幅:1020px*

![container-1120px](/images/articles/tailwind-css-cheat-sheet/container-1120px.png)
*横幅:1120px*

![container-1520px](/images/articles/tailwind-css-cheat-sheet/container-1520px.png)
*横幅:1520px*

## 🌱 陰影

```html
<div class="shadow-inner">shadow-inner</div>
<div class="shadow-sm">shadow-sm</div>
<div class="shadow">shadow</div>
<div class="shadow-md">shadow-md</div>
<div class="shadow-lg">shadow-lg</div>
<div class="shadow-xl">shadow-xl</div>
<div class="shadow-2xl">shadow-2xl</div>
```

![shadow](/images/articles/tailwind-css-cheat-sheet/shadow.png)

## 🌱 輪郭

```html
<button class="outline outline-offset-2 outline-1">outline-1</button>
<button class="outline outline-offset-2 outline-2">outline-2</button>
<button class="outline outline-offset-2 outline-4">outline-4</button>
<button class="outline outline-offset-2 outline-8">outline-8</button>
```

![outline](/images/articles/tailwind-css-cheat-sheet/outline.png)
