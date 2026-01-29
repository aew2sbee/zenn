---
title: "自作のUIコンポーネントを登録する"
---

## 🌱 このチャプターのゴール
ローカル環境で`Storybook`を起動し、
下記キャプチャーのように 自作ボタンが表示されるところまで進めます。

![original-button](/images/books/learn-storybook-tutorial/original-button.png)


## 🌱 自作のUIコンポーネントの作成
`color`と`size`を切り替えられる Button コンポーネントを作成します。
（Tailwind CSS のクラスを切り替えることで見た目を変更します）

```tsx: src/client/components/ui/Button/Button.tsx
import * as React from "react";

export type ButtonProps = React.ButtonHTMLAttributes<HTMLButtonElement> & {
  color?: "primary" | "secondary";
  size?: "small" | "medium" | "large";
};

const BASE = "inline-flex items-center justify-center gap-2 rounded-full font-bold leading-none transition select-none";

const colorMap = {
  primary: "bg-sky-400 text-white hover:bg-sky-500 active:bg-sky-600",
  secondary: "border border-slate-300 bg-white text-slate-900 hover:bg-slate-50 active:bg-slate-100",
} as const;

const sizeMap = {
  small: "h-8 px-4 text-xs",
  medium: "h-10 px-5 text-sm",
  large: "h-12 px-6 text-base",
} as const;

export function Button({
  color = "primary",
  size = "medium",
  type = "button",
  className = "",
  children,
  ...props
}: ButtonProps) {
  return (
    <button
      type={type}
      className={`${BASE} ${colorMap[color]} ${sizeMap[size]} ${className}`}
      {...props}
    >
      {children}
    </button>
  );
}

```

## 🌱 エクスポート用のファイルの作成
他の場所から`import`しやすいように、`index.ts`で再エクスポートします。

```tsx: src/client/components/ui/Button/index.ts
export { Button } from "./Button";
export type { ButtonProps } from "./Button";

```

## 🌱 Storybook専用ファイルの作成
`Storybook`に表示するための`*.stories.tsx`を作成します。

```tsx: src/client/components/ui/Button/Button.stories.tsx
import type { Meta, StoryObj } from '@storybook/nextjs-vite';
import { fn } from 'storybook/test';
import { Button } from '.';

const meta = {
  title: "UI/Button",
  component: Button,
  parameters: {
    // Canvas 上でコンポーネントを中央寄せで表示するための任意パラメータ
    // 詳細: https://storybook.js.org/docs/configure/story-layout
    layout: "centered",
  },
  // このコンポーネントには自動生成された Autodocs ページが作成されます
  // 詳細: https://storybook.js.org/docs/writing-docs/autodocs
  tags: ["autodocs"],
  // argTypes の詳細設定（Storybook Controls 用）
  // 詳細: https://storybook.js.org/docs/api/argtypes
  argTypes: {},
  // fn を使って onClick をスパイすることで、
  // クリック時に Actions パネルへイベントが表示されるようになります
  // 詳細: https://storybook.js.org/docs/essentials/actions#action-args
  args: { onClick: fn() },
} satisfies Meta<typeof Button>;

export default meta;
type Story = StoryObj<typeof meta>;

export const ColorPrimary: Story = {
  args: {
    color: "primary",
    children: "ログイン",
  },
};

export const ColorSecondary: Story = {
  args: {
    color: "secondary",
    children: "ログイン",
  },
};

export const SizeLarge: Story = {
  args: {
    size: "large",
    children: "ログイン",
  },
};

export const SizeMedium: Story = {
  args: {
    children: "ログイン",
  },
};

export const SizeSmall: Story = {
  args: {
    size: "small",
    children: "ログイン",
  },
};

```

```bash
npm run storybook
```

## 🌱 Storybookの設定ファイルを変更
`Storybook`が参照する`stories`の対象範囲を設定します。

```diff ts .storybook/main.ts
import type { StorybookConfig } from '@storybook/nextjs-vite';

const config: StorybookConfig = {
  "stories": [
-    "../stories/**/*.stories.@(js|jsx|mjs|ts|tsx)"
+    "../src/**/*.stories.@(js|jsx|ts|tsx)"
  ],
  "addons": [],
  "framework": "@storybook/nextjs-vite",
  "staticDirs": [
    "..\\public"
  ]
};
export default config;

```
また、プレビュー側で`globals.css`を読み込むようにします
（Tailwind の見た目を反映させるためです）。

```diff ts .storybook/preview.ts
import type { Preview } from '@storybook/nextjs-vite'
+ import '../src/app/globals.css'

const preview: Preview = {
  parameters: {
    controls: {
      matchers: {
        color: /(background|color)$/i,
        date: /Date$/,
      },
    },
  },
};

export default preview;

```

## 🌱 storybook/addon-docsの追加
`@storybook/addon-docs`を追加します。
赤枠の通りでドキュメント情報を追加することが出来ます。
便利なので追加します。

![storybook-addon-docs](/images/books/learn-storybook-tutorial/storybook-addon-docs.png)

```bash
npm i -D @storybook/addon-docs
```

:::details ターミナルのログを見る
```bash
$ npm i -D @storybook/addon-docs

added 3 packages, and audited 484 packages in 3s

179 packages are looking for funding
  run `npm fund` for details

found 0 vulnerabilities
```
:::

`.storybook/main.ts`も更新します
```diff ts .storybook/main.ts
import type { StorybookConfig } from '@storybook/nextjs-vite';

const config: StorybookConfig = {
  "stories": [
    "../src/**/*.stories.@(js|jsx|ts|tsx)"
  ],
-  "addons": [],
+  "addons": ["@storybook/addon-docs"],
  "framework": "@storybook/nextjs-vite",
  "staticDirs": [
    "..\\public"
  ]
};
export default config;

```

## 🌱 ローカル環境での起動

```bash
npm run storybook
```

:::details ターミナルのログを見る
```bash
$ npm run storybook

> tech-storybook@0.1.0 storybook
> storybook dev -p 6006


┌  storybook v10.2.1
│
●  Starting...
│ ╭────────────────────────────────────────────────────╮
│ │   Storybook ready!                                 │
│ │                                                    │
│ │   - Local:             http://localhost:6006/      │
│ │   - On your network:   http://10.99.1.170:6006/    │
│ ╰────────────────────────────────────────────────────╯
│
●  240 ms for manager and 692 ms for preview
```
:::

下記キャプチャーのように起動できていれば`OK`です。
![local-start-storybook](/images/books/learn-storybook-tutorial/local-start-storybook.png)
