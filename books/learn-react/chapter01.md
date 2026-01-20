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
:::

## 🌱 参考情報

@[card](https://ja.react.dev/learn#components)