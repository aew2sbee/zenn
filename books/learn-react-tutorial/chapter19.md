---
title: "イベント伝播"
---

## 🌱 イベント伝播（バブリング）
下記コードの`ボタンA`または`ボタンB`をクリックすると、次の順番でアラートが表示されます。

- `ボタンAが押されたよ!` -> `親の<div>が反応するよ`
- `ボタンBが押されたよ!` -> `親の<div>が反応するよ`

:::message
**ポイント**
クリックした要素（button）の`onClick`が先に実行され、
そのあと親要素へイベントが伝わり（バブリング）、親の`<div>`の`onClick`が実行されます。
:::

```mermaid
flowchart BT
  A[button（クリックした要素）] --> B[div.Toolbar（親）]
  B --> C[body]
  C --> D[document]

  A -. 発火順 .-> A
  A -. 次に親へ .-> B
  B -. さらに上へ .-> C
  C -. さらに上へ .-> D
```

```tsx
"use client";

export default function Toolbar() {
  return (
    <div className="Toolbar" onClick={() => {
      alert('親の<div>が反応するよ');
    }}>
      <button onClick={() => alert('ボタンAが押されたよ!')}>
        ボタンA
      </button>
      <button onClick={() => alert('ボタンBが押されたよ!')}>
        ボタンB
      </button>
    </div>
  );
}

```

## 🌱 伝播（バブリング）を止める
親コンポーネントにイベントを伝えたくない場合は、クリックハンドラ内で`e.stopPropagation()`を呼び出します。
ここでの`e`はイベントオブジェクトで、クリックに関する情報を持っています（引数名は`e`でも`event`でもOKです）。

以下のように`Button`コンポーネント側で`stopPropagation()`を呼ぶと、親の`<div>`の`onClick`は実行されません。

```mermaid
flowchart BT
  A[button] --> B["div.Toolbar"]
  B --> C[body]
  C --> D[document]

  A -. "onClick（button）" .-> A
  A -. "stopPropagation() でここで終了" .-> A
  B -. "div.Toolbar の onClick は呼ばれない" .-> B
```

```diff tsx
"use client";
+ import { ReactNode } from "react";
+
+ function Button({
+   onClick,
+   children,
+ }: {
+   onClick: () => void;
+   children: ReactNode;
+ }) {
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
+      </Button>
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


## 🌱 伝播と stopPropagation の注意点

子の`<button>`をクリックすると、自動で親の`onClick`まで発火します。これには次の注意点があります。

:::message
**ポイント**
- 親が意図せず反応してしまう（子は“自分だけ”反応してほしいのに、親の処理も走る）
- あとから親に`onClick`を足したら、子のクリック挙動が変わる（影響範囲が広がる）
- どのクリックがどの処理を動かしているか追跡しづらい（バグ調査で「なんで親が動いた？」になりやすい）

伝播に頼った設計は、影響範囲が読みにくくなります。
問題が起きるたびに`stopPropagation()`で塞ぐとその場しのぎになりやすいため、親で処理したい場合は props でハンドラを渡し、子から明示的に呼び出すなど、ハンドラの配置を見直します。
:::

## 🌱 デフォルト動作を防ぐ（preventDefault）

`<form>`の`submit`は、送信ボタンが押されるとデフォルトでページ遷移（リロード）を伴うことがあります。
（SPA（Single Page Application：ページ全体を再読み込みせずに画面を切り替えるアプリ）ではこれが邪魔になることが多いです）

```jsx
"use client";

export default function Signup() {
  return (
    <form onSubmit={() => alert('Submitting!')}>
      <input />
      <button>Send</button>
    </form>
  );
}

```

イベントオブジェクトの`e.preventDefault()`を呼び出して、これを防げます。
```diff jsx
"use client";

export default function Signup() {
  return (
-    <form onSubmit={() => alert('Submitting!')}>
+    <form onSubmit={e => {
+      e.preventDefault();
+      alert('Submitting!');
+    }}>
      <input />
      <button>Send</button>
    </form>
  );
}

```

:::message
**ポイント**
- `e.stopPropagation()`: ツリーの上側にあるタグにアタッチされたイベントハンドラが発火しないようにします。
- `e.preventDefault()`: フォーム送信時のページ遷移など、一部のイベントが持つブラウザ標準の動作を止めます。
:::

## 🌱 参考
- https://ja.react.dev/learn/responding-to-events
