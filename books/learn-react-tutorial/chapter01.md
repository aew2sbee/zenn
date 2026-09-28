---
title: "コンポーネント"
---

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
TypeScript で JSX（<p>...</p> のような記法）を含むファイルは、拡張子を .tsx にします。

:::message alert
**ポイント**
`.tsx` -> `.ts`に変えると JSX を TypeScript として解釈できず、ビルドエラーになります。

```bash
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
- React のコンポーネント名は 大文字で始める必要があります
- `<MyButton />`が大文字で始まっているのは「React コンポーネント」を表すためです
- `<button>`のように 小文字で始まるものは HTML タグとして扱われます
:::


## 🌱 JSXで self-closing（/>）が必要なタグ
JSX では、子要素を持たない要素は必ず /> で閉じます。

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

メタ・ドキュメント系
（※ 通常は React コンポーネント内では使わない）
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


## 🌱 TSXの構文（Fragment と div）
JSX では return の中で要素を複数並べたいとき、必ず1つの親要素で包む必要があります。
その親要素として、<div> の代わりに **Fragment（<>...</>）**を使うこともできます。
<div>...</div> や空の <>...</> ラッパのような共通の親要素で囲む必要があります。

:::message
**ポイント**
- レイアウトや CSS の都合で “親の div が欲しい” ときは div、不要なら Fragment が便利です。

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
