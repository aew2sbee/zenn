---
title: '[知識] 画像表示(Imageコンポーネント)'
free: true
---

## Image コンポーネントとは

> 画像の最適化やパフォーマンスの向上を容易にするためのものです。

#### メリット

1. `width`と`height`を省略する事が出来る
2. 画像の`最適化`をしてくれる
3. リロード時に画像が表示される領域を確保してくれる -> `リロード時のレイアウトが崩れない`

## 画像表示の実装

### 1. 画像表示 [内部ファイル]

```tsx: frontend/src/pages/image.tsx
import { NextPage } from 'next'
import Iamge from 'next/image'
// 画像ファイルをインポート
import Cover from '/public/images/cover.png'

const ImageSample: NextPage<void> = () => {
  return (
    <>
      <p>Iamgeコンポーネントで表示中</p>
      <Iamge src={Cover} alt="書影" />
    </>
  )
}

export default ImageSample

```

### 動作確認

:::message
`http://localhost:3000/image`にアクセスすると下記の画像ように表示される
:::

![nextjs-image-step01](/images/books/nextjs-ts-tutorial/image/nextjs-image-step01.png)

### 2. 画像表示 [外部ファイル]

[Google Books API](https://developers.google.com/books?hl=ja)で取得した画像を表示させたい場合

1. `next.config.js`に外部ドメイン(`'books.google.com'`)を追加する

```diff js: frontend/next.config.js
/** @type {import('next').NextConfig} */
const nextConfig = {
+  images: {
+    domains: ['books.google.com'],
+  },
}

module.exports = nextConfig

```

2. Image コンポーネントの`src`に URL を追加する

```tsx: frontend/src/pages/image.tsx
import { NextPage } from 'next'
import Iamge from 'next/image'

const ImageSample: NextPage<void> = () => {
  const URL = 'http://books.google.com/books/content?id=Wx1dLwEACAAJ&printsec=frontcover&img=1&zoom=1&source=gbs_api'
  return (
    <>
      <p>Iamgeコンポーネントで表示中</p>
      <Iamge src={URL} width={150} height={200}  alt="書影" />
    </>
  )
}

export default ImageSample

```

### 動作確認

:::message
`http://localhost:3000/image`にアクセスすると下記の画像ように表示される
:::

![nextjs-image-step02](/images/books/nextjs-ts-tutorial/image/nextjs-image-step02.png)
