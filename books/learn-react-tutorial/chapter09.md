---
title: "デフォルトエクスポートと名前付きエクスポート"
---

## 🌱 デフォルトエクスポート

:::message
**ポイント**
| 観点           | デフォルトエクスポート | 名前付きエクスポート |
| ------------ | ----------- | ---------- |
| 1ファイルに1つだけ   | ◎ 向いている     | △          |
| 複数の関数・値      | △           | ◎ 向いている    |
| リファクタしやすさ    | △           | ◎          |
| IDE補完・型安全    | △           | ◎          |
| Reactコンポーネント | ○（よく使う）     | ◎（公式推奨寄り）  |

:::

```tsx: Button.tsx
// エクスポート側
export default function Button() {
  return <button>Click</button>;
}
```

```tsx: page.tsx
// インポート側
import Button from './Button';
...
```

## 🌱 名前付きエクスポート

```tsx: Button.tsx
// エクスポート側
export function Button() {
  return <button>Click</button>;
}
```

```tsx: page.tsx
// インポート側
import { Button } from './Button';
...
```

複数エクスポートも可能
```tsx: Button.tsx
// エクスポート側
export function PrimaryButton() {}
export function SecondaryButton() {}
```

```tsx: page.tsx
// インポート側
import { PrimaryButton, SecondaryButton } from './Button';
...
```


## 🌱 React・Next.js での実践的な使い分け
ページ（Next.js）

```tsx: app/page.tsx
export default function Page() {
  return <h1>Hello</h1>;
}
```

再利用コンポーネント
```tsx: components/Button.tsx
// エクスポート側
export function Button() {}
```

```tsx: app/page.tsx
// インポート側
import { Button } from '@/components/Button';

export default function Page() {
  return <h1>Hello</h1>;
}
```

カスタムフック
```ts: hooks/useAuth.ts
// エクスポート側
export function useAuth() {}
```

```tsx: app/page.tsx
// インポート側
import { useAuth } from '@/hooks/useAuth';
```

ユーティリティ
```ts: utils/date.ts
// エクスポート側
export function formatDate() {}
export function parseDate() {}
```
