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