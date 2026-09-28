---
title: "イベントに応答する"
---

## 🌱 イベントハンドラ関数

:::message
**ポイント**
- ステートメント（statement）領域などに**イベントハンドラ関数**を定義する
- コンポーネントが画面に表示する内容（UI）領域では、**イベントハンドラ関数を渡す**
:::

```diff tsx
function MyButton() {
+  function handleClick() {
+    alert('You clicked me!');
+  }

  return (
+    <button onClick={handleClick}>
      Click me
    </button>
  );
}

```

:::message alert
**重要**
`handleClick`の末尾に`()`を付けない
「関数を実行している」のではなく、「関数そのものを渡している」から
イベント名の先頭に`handle`が付いた名前にする
:::

```diff tsx
function MyButton() {
  function handleClick() {
    alert('You clicked me!');
  }

  return (
      // 型 'void' を型 'MouseEventHandler<HTMLButtonElement> | undefined' に割り当てることはできません。
-    <button onClick={handleClick()}>
+    <button onClick={handleClick}>
      Click me
    </button>
  );
}

```

## 🌱 アロー関数
アロー関数で書くこともできます

```diff tsx
- <button onClick={function handleClick() {
-   alert('You clicked me!');
+ <button onClick={() => {
+   alert('You clicked me!');
}}>

```

## 🌱 イベントハンドラの props の命名
親から子へ値や関数を渡す仕組みを props と呼びます（詳細は「コンポーネントに props を渡す」の章）。
イベントハンドラの`props`名は`on`で始め、次に大文字が続くようにする

```tsx
// Button コンポーネントの props である onClick は onSmash と命名することも可能
return (
  <button onClick={onSmash}>
    {children}
  </button>
);

```

## 🌱 参考
- https://ja.react.dev/learn/responding-to-events
