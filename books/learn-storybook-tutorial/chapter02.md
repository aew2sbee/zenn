---
title: "Storybookのインストール"
---

## 🌱 インストール

```bash
npm create storybook@latest
```

```bash
$ npm create storybook@latest

> tech-storybook@0.1.0 npx
> create-storybook


┌  Initializing Storybook
│
●  Adding Storybook version 10.2.0 to your project
│
◇  Framework detected: nextjs-vite
│
◆  New to Storybook?
│  ● Yes: Help me with onboarding
│  ○ No: Skip onboarding & don't ask again
└
```

:::message
**翻訳*
> ◆  New to Storybook?
> │  ● Yes: Help me with onboarding
> │  ○ No: Skip onboarding & don't ask again

Storybookは初めて使いますか？
- Yes: Storybook初心者向けの案内（オンボーディング）を表示しますか？
- No: オンボーディングは不要。今後も聞かなくてOK
:::



```bash

$ npm create storybook@latest

> tech-storybook@0.1.0 npx
> create-storybook


┌  Initializing Storybook
│
●  Adding Storybook version 10.2.0 to your project
│
◇  Framework detected: nextjs-vite
│
◇  New to Storybook?
│  Yes: Help me with onboarding
│
●  Storybook collects completely anonymous usage telemetry. We use it to shape
│  Storybook's roadmap and prioritize features. You can learn more, including how
│  to opt out, at https://storybook.js.org/telemetry
│
◆  Storybook configuration generated
│
│  - Configuring ESLint plugin
│  - Configuring main.ts
│  - Configuring preview.ts
│  - Adding Storybook command to package.json
│  - Copying framework templates
│
◆  Dependencies added to package.json
│
│  Adding devDependencies:
│  - storybook@^10.2.0
│  - @storybook/nextjs-vite@^10.2.0
│  - @chromatic-com/storybook@^5.0.0
│  - @storybook/addon-vitest@^10.2.0
│  - @storybook/addon-a11y@^10.2.0
│  - @storybook/addon-docs@^10.2.0
│  - @storybook/addon-onboarding@^10.2.0
│  - vite@^7.3.1
│  - eslint-plugin-storybook@^10.2.0
│  - vitest
│  - playwright
│  - @vitest/browser-playwright
│  - @vitest/coverage-v8
│
◇  Dependencies installed
│
◆  Addons configured successfully
│
│  ✅ @chromatic-com/storybook
│  ✅ @storybook/addon-vitest
│  ✅ @storybook/addon-a11y
│  ✅ @storybook/addon-docs
│  ✅ @storybook/addon-onboarding
│
│  Playwright browser binaries are necessary for @storybook/addon-vitest. The
│  download can take some time. If you don't want to wait, you can skip the
│  installation and run the following command manually later:
│  npx playwright install chromium --with-deps
│
◆  Do you want to install Playwright with Chromium now?
│  ● Yes / ○ No
└
```


:::message
**翻訳*
> ◆  Do you want to install Playwright with Chromium now?
> │  ● Yes / ○ No

Playwright（Chromium）を今すぐインストールしますか？
- Yes: 今すぐ chromium ブラウザをダウンロード＆セットアップする
- No: 今はインストールしない
:::


```bash
◇  Playwright browser binaries installed successfully
│
◇  Storybook was successfully installed in your project!
│
│  To run Storybook manually, run npm run storybook. CTRL+C to stop.
│
│  Wanna know more about Storybook? Check out https://storybook.js.org/
│  Having trouble or want to chat? Join us at https://discord.gg/storybook/
No Instance(s) Available.
│
└


┌  storybook v10.2.0

```