---
title: '[Tailwind CSS] 私なりのチートシート' # 記事のタイトル
emoji: '🍃' # アイキャッチとして使われる絵文字（1文字だけ）
type: 'tech' # tech: 技術記事 / idea: アイデア
topics: ['css', 'tailwindcss', '初心者向け'] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開設定（falseにすると下書き）
---

## はじめに

この記事では、**基本的な Tailwind CSS**を解説します。
※ 随時更新予定です。

:::details 参考資料
@[card](https://gihyo.jp/book/2024/978-4-297-13943-8)
:::

## 0. 前提条件

1. 下記サイトを活用して表示しております。
   @[card](https://play.tailwindcss.com/)

2. 見やすいように`font-size`を`200%`にしております。

```css
html {
  font-size: 200%;
}
```

3. 見やすいように下記`div`で囲っております。

```html
<div class="flex flex-col justify-center min-h-screen mx-10">
  <!-- 対象のhtmlを記載 -->
</div>
```

4. 見やすいように**背景色(`bg-cyan-X00`)** を設定している場合があります。

## 1. 背景

### 1. 背景色を設定する

```html
<p class="">backgroundColor未設定</p>
<p class="bg-cyan-500">backgroundColorをrgb(6, 182, 212)に設定</p>
```

![bk](/images/articles/tailwind-css-cheat-sheet/bk.png)

### 2. 背景色を不透明度を設定する

```html
<p class="bg-cyan-500">backgroundColorをrgb(6, 182, 212)に設定</p>
<p class="bg-cyan-500/50">backgroundColorをrgb(6, 182, 212, 0.5)に設定</p>
<p class="bg-cyan-500/30">backgroundColorをrgb(6, 182, 212, 0.3)に設定</p>
```

![bk-opacity](/images/articles/tailwind-css-cheat-sheet/bk-opacity.png)

## 2.文字

### 1. 文字色を設定する

```html
<p class="">textColor未設定</p>
<p class="text-red-500">textColorをrgb(239, 68, 68)に設定</p>
```

![text-color](/images/articles/tailwind-css-cheat-sheet/text-color.png)

### 2. 文字色を不透明度を設定する

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
<p class="bg-cyan-300 text-lg">
  font-size: 1.125rem; line-height: 1.75rem;
</p>
<p class="bg-cyan-400 text-xl">font-size: 1.25rem; line-height: 1.75rem;</p>
<p class="bg-cyan-500 text-2xl">font-size: 1.5rem; line-height: 2rem;</p>
```

![text-size](/images/articles/tailwind-css-cheat-sheet/text-size.png)

### 4. 文字の高さを設定する

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
<p class="font-blod">font-weight: 700</p>
<p class="font-black">font-weight: 900</p>
```

![font-weight](/images/articles/tailwind-css-cheat-sheet/font-weight.png)

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
| text-align: justify | TD | 依存しない | 美観が求められる文章配置向け |
| text-align: start | 書字方向の開始側揃え | 依存する | 国際化対応、動的言語切替に適している |

:::

### 7. 数値表示設定
:::message
数字を等幅フォント（すべての数字が同じ幅）で表示する設定です。
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
  <p class="line-clamp-1 pl-3">
    ああああああああああああああああああああああああああああああああああああああああ
  </p>
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

## 3. margin/pading
### 1. 四方にスペーシングを設定する

```html
<div class="bg-cyan-200 m-6">
  <div class="h-20 w-20">margin: 24px</div>
</div>
<div class="bg-rose-200 p-6">
  <div class="h-20 w-20">pading: 24px</div>
</div>
```
![m-p](/images/articles/tailwind-css-cheat-sheet/m-p.png)

### 2. 上側にスペーシングを設定する
```html
<div class="bg-cyan-200 mt-6">
  <div class="h-20 w-20">margin: 24px</div>
</div>
<div class="bg-rose-200 pt-6">
  <div class="h-20 w-20">pading: 24px</div>
</div>
```
![m-p-t](/images/articles/tailwind-css-cheat-sheet/m-p-t.png)


### 3. 右側にスペーシングを設定する
```html
<div class="bg-cyan-200 mr-6">
  <div class="h-20 w-20">margin: 24px</div>
</div>
<div class="bg-rose-200 pr-6">
  <div class="h-20 w-20">pading: 24px</div>
</div>
```
![m-p-r](/images/articles/tailwind-css-cheat-sheet/m-p-r.png)

### 4. 下側にスペーシングを設定する
```html
<div class="bg-cyan-200 mb-6">
  <div class="h-20 w-20">margin: 24px</div>
</div>
<div class="bg-rose-200 pb-6">
  <div class="h-20 w-20">pading: 24px</div>
</div>
```
![m-p-b](/images/articles/tailwind-css-cheat-sheet/m-p-b.png)

### 5. 左側にスペーシングを設定する
```html
<div class="bg-cyan-200 ml-6">
  <div class="h-20 w-20">margin: 24px</div>
</div>
<div class="bg-rose-200 pl-6">
  <div class="h-20 w-20">pading: 24px</div>
</div>
```
![m-p-l](/images/articles/tailwind-css-cheat-sheet/m-p-l.png)

### 6. 左右側にスペーシングを設定する
```html
<div class="bg-cyan-200 mx-6">
  <div class="h-20 w-20">margin: 24px</div>
</div>
<div class="bg-rose-200 px-6">
  <div class="h-20 w-20">pading: 24px</div>
</div>
```
![m-p-x](/images/articles/tailwind-css-cheat-sheet/m-p-x.png)

### 7. 上下側にスペーシングを設定する
```html
<div class="bg-cyan-200 my-6">
  <div class="h-20 w-20">margin: 24px</div>
</div>
<div class="bg-rose-200 py-6">
  <div class="h-20 w-20">pading: 24px</div>
</div>
```
![m-p-y](/images/articles/tailwind-css-cheat-sheet/m-p-y.png)

### 8. 内側にスペーシングを設定する
```html
<div class="bg-cyan-200 -m-6">
  <div class="h-20 w-20">margin: 24px</div>
</div>
<div class="bg-rose-200 -p-6">
  <div class="h-20 w-20">pading: 24px</div>
</div>
```
![-m-p](/images/articles/tailwind-css-cheat-sheet/-m-p.png)


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

## 4. ボーダー
### 1. 境界線を設定する
```html
<div class="h-20 w-20 bg-cyan-200">境界線なし</div>
<div class="h-20 w-20 bg-rose-200 border border-indigo-500">境界線あり<br>1px solid</div>
```
![border](/images/articles/tailwind-css-cheat-sheet/border.png)

### 2. 境界線の太く設定する
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
<div class="h-10 w-20 bg-cyan-200 border-indigo-500 border-4 border-dashed">点線</div>
<div class="h-10 w-20 bg-cyan-200 border-indigo-500 border-4 border-dotted">ドット</div>
<div class="h-10 w-20 bg-cyan-200 border-indigo-500 border-4 border-double">二重線</div>
<div class="h-10 w-20 bg-cyan-200 border-indigo-500 border-4 border-hidden">非表示</div>
```
![border-style](/images/articles/tailwind-css-cheat-sheet/border-style.png)

## 5. 横幅を設定する
### 1. 横幅を割合で設定する

```html
<div class="flex mt-2 mx-10 bg-gray-50">
  <div class="h-20 bg-cyan-200 w-2/5">40%</div>
  <div class="h-20 bg-rose-200 w-3/5">60%</div>
</div>
```
![w-percent](/images/articles/tailwind-css-cheat-sheet/w-percent.png)

### 2. w-fullとw-auto横幅をで設定する

```html
<div class="flex mt-2 mx-10 bg-gray-50">
  <div class="h-20 bg-cyan-200 w-full">w-full: 親要素いっぱいに広がる</div>
  <div class="h-20 bg-rose-200 w-auto">w-auto: 内容やコンテンツのサイズに応じて広がる</div>
</div>
```
![w-auto-fill](/images/articles/tailwind-css-cheat-sheet/w-auto-fill.png)

## 6. 縦幅を設定する
### 1. 縦幅を割合で設定する

```html
<div class="flex mt-2 mx-10 bg-gray-50">
  <div class="h-20 bg-cyan-200 w-2/5">40%</div>
  <div class="h-20 bg-rose-200 w-3/5">60%</div>
</div>
```
![w-percent](/images/articles/tailwind-css-cheat-sheet/w-percent.png)

### 2. w-fullとw-auto縦幅をで設定する

```html
<div class="flex mt-2 mx-10 bg-gray-50">
  <div class="h-20 bg-cyan-200 w-full">w-full: 親要素いっぱいに広がる</div>
  <div class="h-20 bg-rose-200 w-auto">w-auto: 内容やコンテンツのサイズに応じて広がる</div>
</div>
```
![w-auto-fill](/images/articles/tailwind-css-cheat-sheet/w-auto-fill.png)

## 7. 角を設定する
### 1. 縦幅を割合で設定する

```html
<div class="w-20 bg-cyan-100 rounded">4px</div>
<div class="w-20 bg-cyan-200 rounded-md">6px</div>
<div class="w-20 bg-cyan-300 rounded-lg">8px</div>
<div class="w-20 bg-cyan-400 rounded-2xl">12px</div>
```
![rounded](/images/articles/tailwind-css-cheat-sheet/rounded.png)
