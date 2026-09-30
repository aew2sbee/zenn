---
title: "フックの使用"
---

## 🌱 フックのルール
フックとは、`useState` のように `use` で始まる React の関数のことです。
フックはコンポーネント関数の一番外側（トップレベル）でのみ呼び出せます。

:::message
**ポイント**
- **「フックはトップレベルで」** は、「if・ループ・ネストした関数の中や、早期 return の後で呼ばない」という意味です
- 迷ったら、フックを使う部分を子コンポーネントに分けるのが一番安全で読みやすいです

:::

## 🌱 条件分岐の中で`useState`を呼ぶのはNG
`React`のフック（例: `useState`）は、毎回レンダーされるたびに同じ順番で呼ばれる必要があります。
ところが、`if (showEditor) `の中で`useState`を呼ぶと、
- `showEditor = false`のとき →`useState`は呼ばれない
- `showEditor = true`のとき →`useState`が急に呼ばれる

となり、呼び出し順が変わってしまいます。これが「条件分岐の中でフックを呼ぶのがダメ」な理由です。

```tsx
"use client";
import { useState } from "react";

export default function App() {
  const [showEditor, setShowEditor] = useState(false);

  // NG: 条件の中でフックを呼ぶ
  if (showEditor) {
    const [text, setText] = useState("こんにちは");
    return (
      <div>
        <button onClick={() => setShowEditor(false)}>閉じる</button>
        <input value={text} onChange={(e) => setText(e.target.value)} />
      </div>
    );
  }

  return (
    <div>
      <button onClick={() => setShowEditor(true)}>エディタを開く</button>
    </div>
  );
}
```

下記のように修正する

- `App`は常に同じ順番で`useState`を呼ぶ
- `Editor`は「表示されたときだけ」コンポーネントごと登場するので、`Editor`内の`useState`は常にトップレベルで呼ばれる

```diff tsx
"use client";
import { useState } from "react";

export default function App() {
  const [showEditor, setShowEditor] = useState(false);

-  if (showEditor) {
-    const [text, setText] = useState("こんにちは");
-    return (
-      <div>
-        <button onClick={() => setShowEditor(false)}>閉じる</button>
-        <input value={text} onChange={(e) => setText(e.target.value)} />
-      </div>
-    );
-  }
-
-  return (
-    <div>
-      <button onClick={() => setShowEditor(true)}>エディタを開く</button>
-    </div>
+  return (
+    <div>
+      <button onClick={() => setShowEditor((v) => !v)}>
+        {showEditor ? "閉じる" : "エディタを開く"}
+      </button>
+
+      {showEditor && <Editor />}
+    </div>
+  );
+}
+
+function Editor() {
+  // OK: コンポーネントのトップレベルでフック
+  const [text, setText] = useState("こんにちは");
+
+  return (
+    <div style={{ marginTop: 8 }}>
+      <input value={text} onChange={(e) => setText(e.target.value)} />
+      <p>入力: {text}</p>
+    </div>
  );
}
```

## 🌱 ループ（map）の中で`useState`を呼ぶのはNG
`map`の中で`useState`を呼ぶと、配列の要素数や順序が変わったときに
**フックの呼び出し回数や順番が変わる可能性**があります。

たとえば、要素の追加・削除・並び替えが起きると、React は「この state はどの行のもの？」を正しく対応づけできなくなります。

```tsx
"use client";
import { useState } from "react";

export default function App() {
  const items = ["りんご", "みかん", "ぶどう"];

  return (
    <ul>
      {items.map((name) => {
        // NG: map（ループ）の中でフック
        const [checked, setChecked] = useState(false);

        return (
          <li key={name}>
            <label>
              <input
                type="checkbox"
                checked={checked}
                onChange={() => setChecked((v) => !v)}
              />
              {name}
            </label>
          </li>
        );
      })}
    </ul>
  );
}
```

`map`の中ではコンポーネントを並べるだけにして、`useState`は`ItemRow`のトップレベルに置きます。

```diff tsx
"use client";
import { useState } from "react";

export default function App() {
  const items = ["りんご", "みかん", "ぶどう"];

  return (
    <ul>
-      {items.map((name) => {
+      {items.map((name) => (
-        const [checked, setChecked] = useState(false);
-
-        return (
-          <li key={name}>
-            <label>
-              <input
-                type="checkbox"
-                checked={checked}
-                onChange={() => setChecked((v) => !v)}
-              />
-              {name}
-            </label>
-          </li>
-        );
-      })}
-    </ul>
+        <ItemRow key={name} name={name} />
+      ))}
+    </ul>
+  );
+}
+
+ function ItemRow({ name }: { name: string }) {
+   const [checked, setChecked] = useState(false);
+
+   return (
+     <li>
+       <label>
+         <input
+           type="checkbox"
+           checked={checked}
+           onChange={() => setChecked((v) => !v)}
+         />
+         {name} {checked ? "✅" : ""}
+       </label>
+     </li>
  );
}
```

## 🌱 参考
- https://ja.react.dev/reference/rules/rules-of-hooks
