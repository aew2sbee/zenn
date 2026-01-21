---
title: "データの表示について"
---

## 🌱 変数を表示するには
JSX では、波括弧 {} を使うことで JavaScript の変数や式を埋め込み、画面に表示できます。
HTML の中に JavaScript を差し込める、というイメージを持つと理解しやすいです。


:::message
**ポイント**
{ ... } の中に入った瞬間、HTML ではなく JavaScript の世界になります。

:::

```diff tsx
# user.nameを表示するには
# {user.name} は JavaScript の変数
return (
  <h1>
+    {user.name}
  </h1>
);

```

## 🌱 JSX の属性に変数を渡す
JSX の属性（`className` や `src` など）にも、`{}` を使って `JavaScript` の値を渡せます。

```diff tsx
# className="avatar" は CSS クラスとして "avatar" 文字列を渡す
# src={user.imageUrl} は JavaScript の user.imageUrl 変数の値を読み込み、その値を src 属性として渡します
return (
  <img
+    className="avatar"
+    src={user.imageUrl}
  />
);

```

## 🌱 style 属性にオブジェクトを設定する
`style`属性は、文字列ではなく JavaScript のオブジェクトを受け取ります。
そのため、`{}`を二重に書く形になります。

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

