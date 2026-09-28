---
title: "自作のUIコンポーネントを登録する"
---

## 🌱 このチャプターのゴール
ローカル環境で`Storybook`を起動し、
下記キャプチャーのように自作ボタンが表示されるところまで進めます。

![original-button](/images/books/learn-storybook-tutorial/original-button.png)


## 🌱 サンプルのストーリーを削除する
`Storybook`のインストール時に、サンプルのストーリー（`Button`、`Header`、`Page`）が`src/stories/`に作成されています。
自作のボタンだけを表示するため、フォルダごと削除します。

```bash
# macOS / Linux
rm -rf src/stories
# Windows（PowerShell）
Remove-Item -Recurse -Force src/stories
```

## 🌱 Storybookの設定ファイルを変更
コンポーネントを作成する前に、`Storybook`の設定を変更します。
`Storybook`を起動している場合は、`Ctrl+C`で停止してから進めてください。

### `@storybook/addon-docs`の追加
addonは、`Storybook`に機能を追加するプラグインです。
`@storybook/addon-docs`を入れると、後で作成する`*.stories.tsx`に`tags: ["autodocs"]`を付けたコンポーネントについて、propsの一覧や各ストーリーをまとめたドキュメントページ（Docs）が自動で作成されます。
下図の赤枠のように、コンポーネントの使い方を1ページで確認できて便利なので追加します。
（`Minimal`構成には含まれていないため、自分で追加する必要があります）

![storybook-addon-docs](/images/books/learn-storybook-tutorial/storybook-addon-docs.png)

`Storybook`本体とバージョンをそろえてインストールします。
本体のバージョンは`package.json`の`storybook`で確認できます（本書では`10.2.1`）。

```bash
npm i -D @storybook/addon-docs@10.2.1
```

詳細は公式ドキュメントを参照してください。
@[card](https://storybook.js.org/docs/writing-docs/autodocs)

### `.storybook/main.ts`の変更
追加したaddonを`addons`に設定します。

```diff ts:.storybook/main.ts
import type { StorybookConfig } from '@storybook/nextjs-vite';

const config: StorybookConfig = {
  "stories": [
    "../src/**/*.stories.@(js|jsx|mjs|ts|tsx)"
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

`stories`は、`Storybook`がストーリーとして読み込むファイルのパターンです。
`../src/**/*.stories.@(js|jsx|mjs|ts|tsx)`は、`src`配下のすべての階層にある`〇〇.stories.tsx`などのファイルという意味です。
`src`フォルダがある状態でインストールしたため、最初から`src`配下が対象になっています。

:::message
**ポイント**
`"..\\public"`はWindowsで生成した場合の値です。macOSやLinuxでは、最初から`"../public"`になっています。
このままでもローカル環境では動作します。GitHub Actionsでビルドする際の修正は、チャプター「GitHub Pagesにデプロイする」で行います。
:::

### `.storybook/preview.ts`の変更
`Tailwind CSS`のスタイルを反映させるため、プレビュー側で`globals.css`を読み込むようにします。

```diff ts:.storybook/preview.ts
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

## 🌱 自作のUIコンポーネントを作成する
`src/client/components/ui/Button/`フォルダを作成し、以下の3ファイルを作成します。
（フォルダ構成は個人的な好みです。任意の場所で構いません）

| ファイル | 役割 |
|---|---|
| `Button.tsx` | ボタンのコンポーネント本体 |
| `index.ts` | 他の場所から`import`するためのファイル |
| `Button.stories.tsx` | `Storybook`に表示するためのファイル |

### `Button.tsx`
`color`と`size`を切り替えられるButtonコンポーネントを作成します。
（`Tailwind CSS`のクラスを切り替えることで見た目を変更します）

```tsx:src/client/components/ui/Button/Button.tsx
import * as React from "react";

export type ButtonProps = Omit<React.ButtonHTMLAttributes<HTMLButtonElement>, "color"> & {
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

`ButtonHTMLAttributes`を合成することで、`onClick`や`disabled`など通常のbutton属性もそのまま受け取れるようにしています。
`ButtonHTMLAttributes`には（継承元の`HTMLAttributes`から）`color`属性が含まれているため、`Omit`で除いてから独自の`color`を定義しています。

### `index.ts`
他の場所から`import`しやすいように、`index.ts`で再エクスポートします。
フォルダを指定して`import`すると、そのフォルダの`index.ts`が読み込まれます。

```ts:src/client/components/ui/Button/index.ts
export { Button } from "./Button";
export type { ButtonProps } from "./Button";

```

### `Button.stories.tsx`
`Storybook`に表示するための`*.stories.tsx`を作成します。
ストーリーとは、コンポーネントの表示パターン1つ分のことです。

```tsx:src/client/components/ui/Button/Button.stories.tsx
import type { Meta, StoryObj } from '@storybook/nextjs-vite';
import { fn } from 'storybook/test';
import { Button } from '.';

const meta = {
  title: "UI/Button",
  component: Button,
  parameters: {
    // Canvas（ストーリーが表示される中央の領域）でコンポーネントを中央寄せで表示する
    // 詳細: https://storybook.js.org/docs/configure/story-layout
    layout: "centered",
  },
  // Docsページ（Autodocs）を自動生成する
  // 詳細: https://storybook.js.org/docs/writing-docs/autodocs
  tags: ["autodocs"],
  // Controlsタブの表示を細かく設定する（今回は設定しないため空のまま）
  // 詳細: https://storybook.js.org/docs/api/arg-types
  argTypes: {},
  // クリックをActionsタブに記録する
  // 詳細: https://storybook.js.org/docs/essentials/actions
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

- `import { Button } from '.'`: `.`は同じフォルダを指し、そのフォルダの`index.ts`が読み込まれます
- `export default meta`: このファイル全体の共通設定です。`component`で対象のコンポーネントを、`title`でサイドバーに表示する名前（`UI/Button`）を指定します
- `export const 名前: Story`: ストーリー1つ分です。`args`はコンポーネントに渡すpropsで、`ColorPrimary`などの名前がサイドバーに並びます
- `Meta`・`StoryObj`: `Storybook`が用意している型です。`satisfies Meta<typeof Button>`を付けると、`args`にButtonのprops以外を書いたときに型エラーで気付けます
- `fn()`: クリックされたことを記録する関数です。`Storybook`画面下部の`Actions`タブで、クリックの履歴を確認できます

## 🌱 ローカル環境での起動

```bash
npm run storybook
```

:::details ターミナルのログを見る
```text
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
│ ╰────────────────────────────────────────────────────╯
│
●  240 ms for manager and 692 ms for preview
```
:::

下記キャプチャーのように起動できていればOKです。
画面下部の`Controls`タブでは、propsの値を変えて見た目を試せます。

![local-start-storybook](/images/books/learn-storybook-tutorial/local-start-storybook.png)
