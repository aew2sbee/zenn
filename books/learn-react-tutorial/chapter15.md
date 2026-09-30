---
title: "イベントハンドラを props で渡す"
---

## 🌱 イベントへの応答

`React`では、`JSX`にイベントハンドラを追加して、
ユーザの操作（クリック、ホバー、入力など）に反応させることができます。
イベントハンドラは、ユーザの操作時に呼ばれる自作の関数です。

`<button>`のような組み込み要素は`onClick`などのブラウザ標準イベントを受け取れます。
一方で、自作コンポーネントでは`props`としてイベントハンドラを受け取り、
`onPlayMovie`のようにアプリ固有の名前を付けることもできます。

```tsx
"use client";
import { ReactNode } from "react";

export default function App() {
  return (
    <Toolbar
      onPlayMovie={() => alert('Playing!')}
      onUploadImage={() => alert('Uploading!')}
    />
  );
}

function Toolbar({
  onPlayMovie,
  onUploadImage,
}: {
  onPlayMovie: () => void;
  onUploadImage: () => void;
}) {
  return (
    <div>
      <Button onClick={onPlayMovie}>
        Play Movie
      </Button>
      <Button onClick={onUploadImage}>
        Upload Image
      </Button>
    </div>
  );
}

function Button({
  onClick,
  children,
}: {
  onClick: () => void;
  children: ReactNode;
}) {
  return (
    <button onClick={onClick}>
      {children}
    </button>
  );
}

```

## 🌱 参考
- https://ja.react.dev/learn/adding-interactivity
- https://ja.react.dev/learn/responding-to-events
