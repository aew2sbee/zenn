---
title: "データの表示について"
---

## 🌱 変数を表示するには
波括弧を使うことでコード内の変数を埋め込んでユーザに表示することができます。


:::message
**ポイント**
{ ... } の中に入ったら JavaScriptの世界になる

:::

```diff tsx
# user.nameを表示するには
return (
  <h1>
+    {user.name}
  </h1>
);

```

```diff tsx
# className="avatar" は CSS クラスとして "avatar" 文字列を渡す
# src={user.imageUrl} は JavaScript の user.imageUrl 変数の値を読み込み、その値を src 属性として渡します
return (
  <img
    className="avatar"
+    src={user.imageUrl}
  />
);

```

## 🌱 style属性にオブジェクト情報を設定する



```diff tsx
# JavaScripによる文字列結合も可能
return (
    <img
+       style={{
+        width: user.imageSize,
+        height: user.imageSize
+      }}
    />
);

```

