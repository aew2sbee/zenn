---
title: "デフォルトエクスポートと名前付きエクスポート"
---

## 🌱 デフォルトエクスポート

:::message
**ポイント**
| 観点           | デフォルトエクスポート | 名前付きエクスポート |
| ------------ | ----------- | ---------- |
| 1ファイルに1つだけ   | ◎ 向いている     | △          |
| 複数の関数・値      | △（1ファイルに1つまで）           | ◎ 向いている    |
| 名前の変更・検索のしやすさ    | △（インポート側で自由に名前を付けられるため、名前がぶれやすい）           | ◎（インポート名が固定される）          |
| Reactコンポーネント | ○     | ○  |

React 公式では、1ファイルにコンポーネントが1つならデフォルトエクスポート、複数なら名前付きエクスポートがよく使われると説明されており、どちらを使うかは好みの問題とされています。

:::

```tsx:Button.tsx
// エクスポート側
export default function Button() {
  return <button>Click</button>;
}
```

```tsx:page.tsx
// インポート側
import Button from './Button';
...
```

## 🌱 名前付きエクスポート

```tsx:Button.tsx
// エクスポート側
export function Button() {
  return <button>Click</button>;
}
```

```tsx:page.tsx
// インポート側
import { Button } from './Button';
...
```

複数エクスポートも可能
```tsx:Button.tsx
// エクスポート側
export function PrimaryButton() {}
export function SecondaryButton() {}
```

```tsx:page.tsx
// インポート側
import { PrimaryButton, SecondaryButton } from './Button';
...
```


## 🌱 React・Next.js での実践的な使い分け
ページ（Next.js）

```tsx:app/page.tsx
export default function Page() {
  return <h1>Hello</h1>;
}
```

再利用コンポーネント
```tsx:components/Button.tsx
// エクスポート側
export function Button() {}
```

```tsx:app/page.tsx
// インポート側
import { Button } from '@/components/Button';
// @/ は tsconfig.json で設定したプロジェクトルートの別名

export default function Page() {
  return <h1>Hello</h1>;
}
```

カスタムフック
```ts:hooks/useAuth.ts
// エクスポート側
export function useAuth() {}
```

```tsx:app/page.tsx
// インポート側
import { useAuth } from '@/hooks/useAuth';
```

ユーティリティ
```ts:utils/date.ts
// エクスポート側
export function formatDate() {}
export function parseDate() {}
```

## 🌱 参考
- https://ja.react.dev/learn/importing-and-exporting-components
