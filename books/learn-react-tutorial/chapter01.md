---
title: "コンポーネント"
---

## 🌱 この本について
React 公式サイトのチュートリアル（https://ja.react.dev/learn ）を実際に動かしながら学んだ内容のまとめです。

- 動作確認の環境: Next.js 16.1.3（App Router）+ TypeScript
- サンプルコードは `app/<任意のディレクトリ>/page.tsx` などに置き、`npm run dev` で起動して確認しています
- Next.js の App Router では、`useState` などのフックやイベントハンドラを使うファイルの先頭に `"use client";` が必要です。付けないとサーバーコンポーネントとして扱われ、エラーになります

## 🌱 コンポーネントとは

> コンポーネントは、UI（ユーザインターフェース）を部品として切り出したものです。
> ボタンのような小さな部品にも、ページ全体のような大きな単位にもなります。
> この章では、JS/TS の関数として UI（JSX）を返す「関数コンポーネント」を扱います。

```tsx
// 「Hello world」を表示させるコンポーネント
export default function Home() {
  return (
    <p>Hello world</p>
  );
}

```

## 🌱 拡張子
TypeScript で JSX（`<p>...</p>` のような記法）を含むファイルは、拡張子を `.tsx` にします。

:::message alert
**ポイント**
`.tsx` -> `.ts`に変えると JSX を TypeScript として解釈できず、ビルドエラーになります。

```text
## Error Type
Build Error

## Error Message
Parsing ecmascript source code failed(ECMAScript のソースコードの解析に失敗しました)

## Build Output
./chapter01/app/error/page.ts:3:14
Parsing ecmascript source code failed
  1 | export default function Home() {
  2 |   return (
> 3 |     <p>Hello world</p>
    |              ^^^^^
  4 |   );
  5 | }
  6 |

Expected ',', got 'world'

Next.js version: 16.1.3 (Turbopack)

```

:::

## 🌱 コンポーネントのネスト

```tsx
// 子コンポーネント
function MyButton() {
  return (
    <button>I'm a button</button>
  );
}
```

```tsx
// 親コンポーネント（子を呼び出す）
export default function MyApp() {
  return (
    <div>
      <h1>Welcome to my app</h1>
      <MyButton />
    </div>
  );
}
```

:::message
**ポイント**
- React のコンポーネント名は大文字で始める必要があります
- `<MyButton />`が大文字で始まっているのは「React コンポーネント」を表すためです
- `<button>`のように小文字で始まるものは HTML タグとして扱われます
:::


## 🌱 JSXで self-closing（/>）が必要なタグ
JSX では、`<img>` や `<br>` などの空要素（子要素を持てない要素）は必ず `/>` で閉じます。
子要素を持たない要素も `/>` で閉じられます。

:::message alert
**ポイント**
`/`がないとエラーが発生する
```tsx
export default function Home() {
  return (
    // JSX 要素 'img' には対応する終了タグがありません。
    <img src="/sample.png" >
  );
}
```
:::

改行・区切り系
```tsx
<br />
<hr />
```

画像・メディア系
```tsx
<img src="..." alt="..." />
<source />
<track />
```

フォーム系
```tsx
<input />
<textarea />   // ※ HTMLでは閉じタグが必要だが JSXでは self-closing 可
<option />     // ※ children を持たない場合
```

その他の空要素
```tsx
<meta />
<link />
<base />
<area />
<col />
<embed />
<param />
<wbr />
```


## 🌱 JSXの構文（Fragment と div）
JSX では return の中で要素を複数並べたいとき、`<div>...</div>` などの1つの親要素で包む必要があります。
親要素として、`<div>` の代わりに Fragment（`<>...</>`）を使うこともできます。

:::message
**ポイント**
- レイアウトや CSS の都合で「親の div が欲しい」ときは div、不要なら Fragment が便利です。

:::

```diff tsx
// - HTMLには何も出力されない
// - React 内部だけの「仮の親」
// - DOM を汚さない
function AboutPage() {
  return (
+    <>
      <h1>About</h1>
      <p>Hello there.<br />How do you do?</p>
+    </>
  );
}
```

or

```diff tsx
// - 実際のHTMLに <div> が出力される
// - DOM にノードが1つ増える
// - CSS・レイアウト・アクセシビリティに影響する
function AboutPage() {
  return (
+    <div>
      <h1>About</h1>
      <p>Hello there.<br />How do you do?</p>
+    </div>
  );
}
```

## 🌱 CSS クラスの書き方
JSX では class は予約語のため、代わりに className を使います。

```tsx
<img className="avatar" />
```

```css
.avatar {
  border-radius: 50%;
}
```

## 🌱 参考
- https://ja.react.dev/learn/your-first-component
- https://ja.react.dev/learn/writing-markup-with-jsx
