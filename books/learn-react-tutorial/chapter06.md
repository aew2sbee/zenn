---
title: "画面の更新"
---

## 🌱 コンポーネントに情報を「記憶」させる
コンポーネントに state（状態） を追加すると、値を保持できるようになります。
そして state を更新すると、React が再レンダーを行い、画面表示が更新されます。

:::message
**ポイント**
useState は「現在の値」と「更新用の関数」を 配列で返すので、分割代入で受け取ります。
```tsx
// useState(n): nは初期値
// count: 現在のstateの変数
// setCount: stateを更新するための関数
// useState は「現在の値」と「更新用の関数」を 配列で返すので、分割代入で受け取ります。
const [count, setCount] = useState(0);

```

前の値に依存する更新（関数形式）
```tsx
setCount(c => c + 1)

```

前の値に依存しない更新
```tsx
setCount(10)
setText(input)

```

:::

```diff tsx
"use client"
+ import { useState } from 'react';

export default function MyButton() {
+   const [count, setCount] = useState(0);

  function handleClick() {
+     setCount(c => c + 1);
  }

  return (
    <button onClick={handleClick}>
+       Clicked {count} times
    </button>
  );
}


```

## 🌱 同じコンポーネントを複数の場所でレンダーした場合
同じ`MyButton`を2回配置すると、それぞれが独自の`state`を持ちます。
そのため、片方のボタンをクリックしても、もう片方の`count`には影響しません。

:::message
**ポイント**
`state`は「コンポーネント関数そのもの」ではなく、画面上の各インスタンス（レンダー結果）ごとに保持されます。

:::

```tsx
"use client"
import { useState } from 'react';

export default function MyApp() {
  return (
    <div>
      <h1>Counters that update separately</h1>
      <MyButton />
      <MyButton />
    </div>
  );
}

function MyButton() {
  const [count, setCount] = useState(0);

  function handleClick() {
    setCount(count + 1);
  }

  return (
    <button onClick={handleClick}>
      Clicked {count} times
    </button>
  );
}

```
