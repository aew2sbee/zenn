---
title: '[知識] 画像表示(Imageコンポーネント)'
free: true
---

## Image コンポーネントとは

> 画像の最適化やパフォーマンスの向上を容易にするためのものです。

## 画像表示の実装

### 1. 画像表示

#### メリット

1. `width`と`height`を省略する事が出来る
2. 画像の`最適化`をしてくれる
2. リロード時に画像が表示される領域を確保してくれる -> `リロード時のレイアウトが崩れない`

#### ソースコード

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
