---
title: "propsでコンポーネントを渡す"
---

## 🌱 childrenとしてJSXを渡す
Reactでは、親コンポーネントから子コンポーネントへ値を渡すために`props`（プロパティ）を使います。
その中でも`children`は少し特別で、コンポーネントのタグの間に書いたJSX が自動的に渡される仕組みです。

```tsx
// childrenを受け取る側
import { ReactNode } from "react";

type CardProps = {
  children: ReactNode;
};

export function Card({ children }: CardProps) {
  return <div className="card">{children}</div>;
}
```
- `children`は「表示できる内容（JSX・文字列など）」を受け取るための`props`です
- `{children}`の部分に、親から渡された`JSX`がそのまま描画されます
- `ReactNode`は「画面に表示可能な値」を表す型です

```tsx
// childrenを渡す側
function App() {
  return (
    <Card>
      <h2>タイトル</h2>
      <p>本文です</p>
    </Card>
  );
}
```
ここで <Card> タグの 内側に書かれている`JSX`が、
そのまま`Card`コンポーネントの`children`として渡されます。

---

childrenの中身はどうなっている？
このとき、`Card`の`{children}`には次の`JSX`が入ります。
```tsx
<>
  <h2>タイトル</h2>
  <p>本文です</p>
</>

```
