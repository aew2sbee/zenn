---
title: "イベントに応答する"
---

## 🌱 イベントハンドラ関数

:::message
**ポイント**
ステートメント（statement）領域などに**イベントハンドラ関数** を定義する
コンポーネントが画面に表示する内容（UI）領域に**イベントハンドラ関数を渡す** を定義する
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
`handleClick`の末尾に括弧がいらない
「関数を実行している」のではなく、「関数そのものを渡している」 から
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
イベントハンドラのプロップは`on`で始まり、次に大文字の文字が続くようにする

```tsx
// Button コンポーネントの props である onClick は onSmash と命名することも可能
return (
  <button onClick={onSmash}>
    {children}
  </button>
);

```
