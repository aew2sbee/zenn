---
title: "配列処理について"
---

## 🌱 条件付きレンダー(if文)
React には、条件分岐を書くための特別な構文は存在しない
JavaScript コードを書くときに使うのと同じ手法を使う

:::message
**ポイント**
if文は、基本的にステートメント（statement）領域に記載する

:::

```ts
const products = [
  { title: 'Cabbage', id: 1 },
  { title: 'Garlic', id: 2 },
  { title: 'Apple', id: 3 },
];

```

```ts
const listItems = products.map(product =>
  <li key={product.id}>
    {product.title}
  </li>
);

return (
  <ul>{listItems}</ul>
);
```



## 🌱 コンポーネントが画面に表示する内容（UI）領域の三項演算子(if文)
コンパクトなコードで記載することができます。
```tsx
const listItems = products.map(product =>
  <li key={product.id}>
    {product.title}
  </li>
);

return (
  <ul>{listItems}</ul>
);

```

## 🌱 コンポーネントが画面に表示する内容（UI）領域の論理積(if文)
else側の分岐が不要な条件の時に活躍できます。

```diff tsx
# JavaScripによる文字列結合も可能
return (
  <div>
+    {isLoggedIn && <AdminPanel />}
  </div>
);

```

