---
title: "useState"
---

## 🌱 state：コンポーネントのメモリ

コンポーネントは、現在の入力値や選択中の画像、
ショッピングカートの状態などを「覚えておく」必要があります。

`React`では、このような コンポーネント固有のメモリを`state`と呼びます。

`useState`フックを使用すると、コンポーネントに`state`を追加することができます。

```tsx
const [index, setIndex] = useState(0);
const [showMore, setShowMore] = useState(false);
```

以下は、`state`を使って表示内容を切り替える例です。
```tsx
import { useState } from 'react';
import { sculptureList } from './data.js';

export default function Gallery() {
  const [index, setIndex] = useState(0);
  const [showMore, setShowMore] = useState(false);
  const hasNext = index < sculptureList.length - 1;

  function handleNextClick() {
    if (hasNext) {
      setIndex(index + 1);
    } else {
      setIndex(0);
    }
  }

  function handleMoreClick() {
    setShowMore(!showMore);
  }

  let sculpture = sculptureList[index];
  return (
    <>
      <button onClick={handleNextClick}>
        Next
      </button>
      <h2>
        <i>{sculpture.name} </i>
        by {sculpture.artist}
      </h2>
      <h3>
        ({index + 1} of {sculptureList.length})
      </h3>
      <button onClick={handleMoreClick}>
        {showMore ? 'Hide' : 'Show'} details
      </button>
      {showMore && <p>{sculpture.description}</p>}
      <img
        src={sculpture.url}
        alt={sculpture.alt}
      />
    </>
  );
}

```

## 🌱 “+3” をクリックしても 1 しかスコアが増えない不具合
`state`を更新すると、新しい再レンダーが予約されますが、
すでに実行中のコード内で参照している`state`の値は変わりません。

そのため、以下のコードでは
`setScore(score + 1)`を複数回呼び出しても、
すべて 同じ`score`の値を元に計算されてしまいます。

```tsx
import { useState } from 'react';

export default function Counter() {
  const [score, setScore] = useState(0);

  function increment() {
    setScore(score + 1);
  }

  return (
    <>
      <button onClick={() => increment()}>+1</button>
      <button onClick={() => {
        increment();
        increment();
        increment();
      }}>+3</button>
      <h1>Score: {score}</h1>
    </>
  )
}

```

この問題は、更新用関数を渡すことで解決できます。

```diff tsx
import { useState } from 'react';

export default function Counter() {
  const [score, setScore] = useState(0);

  function increment() {
      // state を設定する際に更新用関数を渡すことでこれを修正することができます。
-    setScore(score + 1);
+    setScore(s => s + 1);
  }

  return (
    <>
      <button onClick={() => increment()}>+1</button>
      <button onClick={() => {
        increment();
        increment();
        increment();
      }}>+3</button>
      <h1>Score: {score}</h1>
    </>
  )
}

```

この書き方では、`React`が 直前の`state`を順番に使って
更新を処理するため、複数回の更新が正しく反映されます。
