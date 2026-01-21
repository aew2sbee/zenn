---
title: "条件式について"
---

## 🌱 条件付きレンダー(if文)
React には、条件分岐専用の特別な構文は存在しません。
その代わり、JavaScript で普段使っている条件分岐の書き方をそのまま利用します。

:::message
**ポイント**
if 文は JSX の中では直接書けないため、
**基本的にステートメント（statement）領域に記載します**

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

## 🌱 UI領域での三項演算子（if文）
条件分岐がシンプルな場合は、**三項演算子**を使うことで
よりコンパクトに記述できます。
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

## 🌱 UI領域での論理積（&&）を使った条件付きレンダー
else 側の処理が不要な場合は、**論理積（AND演算子）**が便利です。

```diff tsx
# JavaScripによる文字列結合も可能
return (
  <div>
+    {isLoggedIn && <AdminPanel />}
  </div>
);

```

