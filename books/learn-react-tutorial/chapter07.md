---
title: "フックの使用"
---

## 🌱 フック (Hook)
`use`で始まる関数のこと

## 🌱 フックはコンポーネントのトップレベルで呼ぶ

```tsx
"use client";
import { useState } from "react";

export default function OkExample() {
  const [count, setCount] = useState(0);

  return <button onClick={() => setCount(count + 1)}>{count}</button>;
}
```


```tsx
"use client";
import { useState } from "react";

export default function NgIfExample({ enabled }: { enabled: boolean }) {
  if (enabled) {
    const [count, setCount] = useState(0);
    return <button onClick={() => setCount(count + 1)}>{count}</button>;
  }
  return <p>disabled</p>;
}
```


```tsx
"use client";
import { useState } from "react";

export default function NgHandlerExample() {
  function handleClick() {
    const [count, setCount] = useState(0);
    setCount(count + 1);
  }

  return <button onClick={handleClick}>click</button>;
}
```

:::message
**ポイント**
フックには通常の関数より多くの制限があります。

1. フックはコンポーネントのトップレベル（または他のフック内）でのみ呼び出すことができます
```tsx
// useState(n): nは初期値
// count: 現在のstateの変数
// setCount: stateを更新するための関数
const [count, setCount] = useState(0);
// useState は「現在の値」と「更新用の関数」を 配列で返すので、分割代入で受け取ります。
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


## 🌱 コンポーネント間でデータを共有する



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
