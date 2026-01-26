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

## 🌱 イベント伝播（バブリング）
下記コードの`ボタンA`または`ボタンB`をクリックすると、次の順番でアラートが表示されます。
- `ボタンAが押されたよ!` -> `親の<div>が反応するよ`
- `ボタンBが押されたよ!` -> `親の<div>が反応するよ`
とポップアップが表示される

:::message
**ポイント**
クリックした要素（button）の`onClick`が先に実行され、
そのあと 親要素へイベントが伝わり（バブリング）、親の`<div>`の`onClick`が実行されます。
:::

```tsx
export default function Toolbar() {
  return (
    <div className="Toolbar" onClick={() => {
      alert('親の<div>が反応するよ');
    }}>
      <button onClick={() => alert('ボタンAが押されたよ!')}>
        ボタンA
      </button>
      <button onClick={() => alert('ボタンBが押されたよ!')}>
        ボタンA
      </button>
    </div>
  );
}

```

伝播を停止するには、
親コンポーネントにイベントを伝えたくない場合は、クリックハンドラ内で`e.stopPropagation()`を呼び出します。
ここでの`e`は イベントオブジェクトで、クリックに関する情報を持っています（引数名は`e`でも`event`でもOKです）。

以下のように`Button`コンポーネント側で`stopPropagation()`を呼ぶと、親の`<div>`の`onClick`は実行されません。

```diff tsx
+ function Button({ onClick, children }) {
+   return (
+     <button onClick={e => {
+       e.stopPropagation();
+       onClick();
+     }}>
+       {children}
+     </button>
+   );
+ }

export default function Toolbar() {
  return (
    <div className="Toolbar" onClick={() => {
      alert('親の<div>が反応するよ');
    }}>
-      <button onClick={() => alert('ボタンAが押されたよ!')}>
+      <Button onClick={() => alert('ボタンAが押されたよ!')}>
        ボタンA
-      </button>
-      <button onClick={() => alert('ボタンBが押されたよ!')}>
+      <Button onClick={() => alert('ボタンBが押されたよ!')}>
        ボタンB
-      </button>
+      </Button>
    </div>
  );
}

```

:::message
**ポイント（実行の流れ）**
1. `React`がクリックされた`<button>`の`onClick`を呼ぶ
2. `Button`内のハンドラが動き、`e.stopPropagation()`でバブリングを止める
3. 続けて`Toolbar`から渡された`onClick`を呼び出し、ボタン固有のアラートを表示する
4. 伝播が止まっているので、親の`<div>`の`onClick`は実行されない
:::
