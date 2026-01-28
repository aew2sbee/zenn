---
title: "レンダーとコミット"
---

## 🌱 レンダーされるタイミング

:::message
**ポイント**
1. コンポーネントの初回呼び出し
2. コンポーネント（またはその親コンポーネント）の`state`の更新。
そのあと 親要素へイベントが伝わり（バブリング）、親の`<div>`の`onClick`が実行されます。
:::

## 🌱 コンポーネントの初回呼び出し

ターゲットとなる`DOM`ノードに対して`createRoot`を呼び出し、作成されたルートの`render`メソッドを、コンポーネントに対して呼び出します。
```jsx
import Image from './Image.js';
import { createRoot } from 'react-dom/client';

const root = createRoot(document.getElementById('root'))
root.render(<Image />);

```

## 🌱 コンポーネントの`state`の更新。

`set`関数を使って`state`を更新することで、さらなるレンダーをトリガすることができます。

:::message
**ポイント**
再レンダー時には、React は前回のレンダーからどの部分が変わったのか、あるいは変わらなかったのかを計算します
:::


https://ja.react.dev/learn/state-as-a-snapshot