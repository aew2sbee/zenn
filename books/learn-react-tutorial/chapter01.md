---
title: "コンポーネントについて"
---

## 🌱 コンポーネントとは

> 独自のロジックと外見を持つ UI（ユーザインターフェース）の部品のことです。
> コンポーネントは、ボタンのような小さなものである場合も、ページ全体を表す大きなものである場合もあります。
> マークアップを返す JavaScript 関数です。

```tsx: chapter01/app/page.tsx
// 「Hello world」を表示させるコンポーネント
export default function Home() {
  return (
    <p>Hello world</p>
  );
}

```

@[card](https://github.com/aew2sbee/tech-react/blob/main/chapter01/app/page.tsx)

## 🌱 拡張子
TypeScriptのコンポーネントの拡張子は、`tsx`です。

:::message alert
**ポイント**
`page.tsx` -> `page.ts`に変えると `Build Error`になる

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
// ネストされるコンポーネント
function MyButton() {
  return (
    <button>I'm a button</button>
  );
}
```

```tsx
// ネストされるコンポーネント
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
React のコンポーネント名は常に大文字で始める必要があります
<MyButton /> が大文字で始まっている
React のコンポーネントであるということを示す
HTML タグは小文字である
:::


## 🌱 `/`が必要になるタグとは

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


## 🌱 TSXの構文
<div>...</div> や空の <>...</> ラッパのような共通の親要素で囲む必要があります。

:::message
**ポイント**
`<>...</>`の場合のメリットがある
- HTMLには何も出力されない
- React 内部だけの「仮の親」
- DOM を汚さない

:::

```diff tsx
# - HTMLには何も出力されない
# - React 内部だけの「仮の親」
# - DOM を汚さない
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
# - 実際のHTMLに <div> が出力される
# - DOM にノードが1つ増える
# - CSS・レイアウト・アクセシビリティに影響する

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

```css
.avatar {
  border-radius: 50%;
}
```

```tsx
<img className="avatar" />
```

