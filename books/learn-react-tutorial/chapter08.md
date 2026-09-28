---
title: "コンポーネント間でデータを共有する"
---

## 🌱 `state`の管理を親コンポーネントへ移す
同じ`count`を複数のボタンで共有したい場合、`state`を子コンポーネント（MyButton）ではなく、共通の親コンポーネント（MyApp）で管理します。
このように`state`を親へ移動することを、よく`state`のリフトアップ（lifting state up）と呼びます。

1. まずは`state`の管理を`MyButton`から`MyApp`に移行する

```diff tsx
"use client";
import { useState } from 'react';

export default function MyApp() {
+  const [count, setCount] = useState(0);
+
+  function handleClick() {
+    setCount(count + 1);
+  }

  return (
    <div>
      <h1>Counters that update separately</h1>
      <MyButton />
      <MyButton />
    </div>
  );
}

function MyButton() {
-  const [count, setCount] = useState(0);
-
-  function handleClick() {
-    setCount(count + 1);
-  }

  return (
    <button onClick={handleClick}>
      Clicked {count} times
    </button>
  );
}

```

2. `MyApp`で`state`を管理するように引数を調整する

親から子へ値や関数を渡す仕組みを props と呼びます（詳細は「コンポーネントに props を渡す」の章）。
```diff tsx
"use client";
import { useState } from 'react';

export default function MyApp() {
  const [count, setCount] = useState(0);

  function handleClick() {
    setCount(count + 1);
  }

  return (
    <div>
      <h1>Counters that update separately</h1>
+      <MyButton count={count} onClick={handleClick} />
+      <MyButton count={count} onClick={handleClick} />
    </div>
  );
}

- function MyButton() {
+ function MyButton({ count, onClick }: { count: number; onClick: () => void }) {

  return (
+    <button onClick={onClick}>
      Clicked {count} times
    </button>
  );
}

```

## 🌱 参考
- https://ja.react.dev/learn
- https://ja.react.dev/learn/sharing-state-between-components
