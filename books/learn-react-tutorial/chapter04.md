---
title: "配列処理について"
---

## 🌱 リストのレンダー
コンポーネントのリストをレンダーする場合は、
`forループ`や配列の`map()関数`といった`JavaScript`の機能を使って行います。

:::message
**ポイント**
`<li>`に`key属性`があることに注意してください。

リスト内の各項目には、兄弟の中でそれを一意に識別するための文字列または数値を渡す必要があります。
通常、`key`はデータから来るはずで、データベース上の ID などが該当します。
`React`は、後でアイテムを挿入、削除、並べ替えることがあった際に、何が起こったかを key を使って把握します。

:::

```tsx
const products = [
  { title: 'Cabbage', isFruit: false, id: 1 },
  { title: 'Garlic', isFruit: false, id: 2 },
  { title: 'Apple', isFruit: true, id: 3 },
];

export default function ShoppingList() {
  const listItems = products.map(product =>
    <li
      key={product.id}
      style={{
        color: product.isFruit ? 'magenta' : 'darkgreen'
      }}
    >
      {product.title}
    </li>
  );

  return (
    <ul>{listItems}</ul>
  );
}


```
