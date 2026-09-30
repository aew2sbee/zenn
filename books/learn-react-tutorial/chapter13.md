---
title: "リストのレンダー"
---

## 🌱 配列を map で表示する
配列データを `<li>` タグで囲んで表示するコンポーネントを作成できます。

```tsx
const people: string[] = [
  'テストA',
  'テストB',
  'テストC',
  'テストD',
  'テストE'
];

export default function List() {
  const listItems = people.map(person =>
    <li>{person}</li>
  );
  return <ul>{listItems}</ul>;
}

```

:::message alert
**ポイント**
`Each child in a list should have a unique "key" prop.`

配列の各アイテムには、`key`を渡す必要があります。
`key`とは、配列内の他のアイテムと区別できるようにするための一意な文字列または数値です。

:::


```diff tsx
- const people: string[] = [
-   'テストA',
-   'テストB',
-   'テストC',
-   'テストD',
-   'テストE'
- ];

+ export const people = [
+   { id: 0, name: "テストA" },
+   { id: 1, name: "テストB" },
+   { id: 2, name: "テストC" },
+   { id: 3, name: "テストD" },
+   { id: 4, name: "テストE" },
+ ];

export default function List() {
-  const listItems = people.map(person =>
-    <li>{person}</li>
+  const listItems = people.map((person) => (
+    <li key={person.id}>{person.name}</li>
-  );
+  ));
  return <ul>{listItems}</ul>;
}

```

## 🌱 key のルール
- データベースの主キーや ID は必然的に一意になるため、その値を`key`として利用できます。

:::message alert
**ポイント**
- 異なる配列に対応する`JSX`ノードには同じ`key`を使用できます。
- `key`はレンダー間で安定している必要があります
  並び替えや追加・削除が起きても同じ要素を同じものとして扱えるため
- `key={Math.random()}`のように、レンダーの最中に`key`を生成してはいけません。
- 項目の順序が変わりうる場合、`index`を`key`にすると並び替えや削除で要素との対応がずれるため避けます。
  データに含まれる`id`のような「安定していて一意な値」を使います。
:::

## 🌱 参考
- https://ja.react.dev/learn/rendering-lists
