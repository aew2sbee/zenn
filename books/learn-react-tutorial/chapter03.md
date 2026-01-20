---
title: "データの表示について"
---

## 🌱 条件付きレンダー(if文)
React には、条件分岐を書くための特別な構文は存在しない
JavaScript コードを書くときに使うのと同じ手法を使う

:::message
**ポイント**
if文は、基本的にステートメント（statement）領域に記載する

:::

```tsx
// --- ▼ ステートメント（statement）領域 ▼ ---
let content;
if (isLoggedIn) {
  content = <AdminPanel />;
} else {
  content = <LoginForm />;
}
// --- ▲ ステートメント（statement）領域 ▲  ---
// --- ▼ コンポーネントが画面に表示する内容（UI）領域 ▼ ---
return (
  <div>
    {content}
  </div>
);
// --- ▲ コンポーネントが画面に表示する内容（UI）領域 ▲  ---

```

## 🌱 コンポーネントが画面に表示する内容（UI）領域の三項演算子(if文)
コンパクトなコードで記載することができます。
```diff tsx
return (
<div>
+  {isLoggedIn ? (
+    <AdminPanel />
+  ) : (
+    <LoginForm />
+  )}
</div>
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

