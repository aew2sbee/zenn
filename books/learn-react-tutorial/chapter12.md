---
title: "条件付きレンダー"
---

## 🌱 条件付きレンダーとは
`React`では、条件分岐（if / 三項演算子など）を`JSX`専用の構文で書く必要はありません。
`JavaScript`の制御フローをそのまま使って表示内容を切り替えるのが基本です。

まずは、シンプルなコンポーネントを見てみましょう。
```tsx
function Item({ name, isPacked }) {
  return <li className="item">{name}</li>;
}
```

## 🌱 if 文で条件分岐する
「梱包済み（`isPacked`が`true`）のときだけチェックマークを表示したい」場合は、
`return`の前で`if`文を使って分岐できます。

```diff tsx
function Item({ name, isPacked }) {
+  if (isPacked) {
+    return <li className="item">{name} ✅</li>;
+  }
  return <li className="item">{name}</li>;
}
```

:::message
**ポイント**
- `return`を複数回書いても問題ありません
- 条件ごとに`JSX`を丸ごと切り替えたいときに向いています
:::


## 🌱 nullを使って何も返さないようにする
条件によっては、「表示自体を消したい」こともあります。
その場合は`null`を返します。

```diff tsx
function Item({ name, isPacked }) {
+ if (isPacked) {
+   return null;
+ }
  return <li className="item">{name}</li>;
}
```

:::message
**ポイント**
- `null`は「レンダーしない」という意味
- エラーではなく、正式な`React`の書き方です
:::

## 🌱 条件 (三項) 演算子`? :`
JSX の中で一部の表示だけを切り替えたい場合は、**三項演算子**が便利です。
```diff tsx
function Item({ name, isPacked }) {
-  if (isPacked) {
-    return <li className="item">{name} ✅</li>;
-  }
-  return <li className="item">{name}</li>;
+ return (
+   <li className="item">
+     {isPacked ? name + ' ✅' : name}
+   </li>
+ );
}
```

:::message
**ポイント**
- JSX の`{}`の中では`JavaScript`の式が書ける
- 表示の一部だけ変えたいときに読みやすい
:::

## 🌱 論理 AND 演算子`&&`
「条件が`true`のときだけ何かを表示したい」場合は、
**論理 AND（&&）**がよく使われます。

```diff tsx
function Item({ name, isPacked }) {
- return <li className="item">{name}</li>;
+ return (
+   <li className="item">
+     {name} {isPacked && '✅'}
+   </li>
+ );
}
```

:::message
**ポイント**
- 条件が`false`である場合、式全体が`false`になります。
- `React`は`false`, `null`, `undefined`を「何もないもの」として扱うため、画面には何も表示されません。
:::

## 🌱 && の左辺に数値を置かない
`&&`を使うときに 初心者がつまずきやすい注意点があります。
```tsx
messageCount && <p>New messages</p>
```
一見問題なさそうですが、`messageCount`が`0`の場合、
- `0`は`falsy`
- しかし 評価結果は`0`
- `React`は`0`を表示してしまう
という挙動になります。

```tsx
// 正しい書き方
messageCount > 0 && <p>New messages</p>

```

:::message
**ポイント**
`&&`の左側には, `true / false`や比較式（`>`, `<`, `===`など）を置くようにすると安全です。
:::
