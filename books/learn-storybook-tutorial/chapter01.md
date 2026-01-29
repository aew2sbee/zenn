---
title: "環境構築"
---

## 🌱 このチャプターのゴール
ローカル環境で、下記のキャプチャーが表示されるところまで進めます。

![installed-successfully-storybook](/images/books/learn-storybook-tutorial/installed-successfully-storybook.png)

## 🌱 Next.jsのインストール
Storybook を動かすために、まずは `Next.js` をインストールします。

```bash
npx create-next-app@latest . --yes
```

:::message
**ポイント**
`Next.js`をインストールするために必要な`Node.js`の導入などは、ここでは解説しません。
あらかじめご自身の環境で準備しておいてください。

:::

:::details ターミナルのログを見る
```bash
$ npx create-next-app@latest . --yes
Creating a new Next.js app in C:\Users\xxxxx\work\tech-storybook.

Using npm.

Initializing project with template: default-tw


Installing dependencies:
- next
- react
- react-dom

Installing devDependencies:
- @tailwindcss/postcss
- @types/node
- @types/react
- @types/react-dom
- eslint
- eslint-config-next
- tailwindcss
- typescript


added 356 packages, and audited 357 packages in 56s

141 packages are looking for funding
  run `npm fund` for details

found 0 vulnerabilities

Generating route types...
✓ Types generated successfully

Success! Created tech-storybook at C:\Users\xxxxx\work\tech-storybook

```
:::

---

`Next.js`が正しく起動するか確認します。

```bash
npm run dev
```

:::details ターミナルのログを見る
```bash
$ npm run dev

> tech-storybook@0.1.0 dev
> next dev

▲ Next.js 16.1.6 (Turbopack)
- Local:         http://localhost:3000
- Network:       http://10.99.1.170:3000

✓ Starting...
✓ Ready in 1375ms
 GET / 200 in 2.6s (compile: 2.2s, render: 331ms)
 GET / 200 in 117ms (compile: 12ms, render: 105ms)
```
:::

ブラウザで`http://localhost:3000`にアクセスし、下記の画面が表示されれば OK です。

![installed-successfully-nextjs](/images/books/learn-storybook-tutorial/installed-successfully-nextjs.png)


## 🌱 `app`ディレクトリを`src`ディレクトリ配下へ移動する(小さなこだわり)

`Next.js`の`app`ディレクトリを`src`配下へ移動します。
これは必須ではありませんが、個人的な好みとして行っています。

```bash
mkdir src
mv app src/

```

```diff bash
.
  ├── public
  ├── node_modules
- └── app
+ └── src
+     └── app

```

## 🌱 Storybookのインストール
続いて`Storybook`をインストールします。
```bash
npm create storybook@latest
```

:::details ターミナルのログを見る
```bash
$ npm create storybook@latest

> tech-storybook@0.1.0 npx
> create-storybook


┌  Initializing Storybook
│
●  Adding Storybook version 10.2.1 to your project
│
◇  Framework detected: nextjs-vite
│
◆  What configuration should we install?
│  ● Recommended: Component development, docs, and testing features.
│  ○ Minimal: Just the essentials for component development.

```
:::


オンボーディング(オプション) のインストール
► ここでは不要なファイルを増やしたくないので`No`を選択します

:::message
```bash
◆  New to Storybook?
│  ● Yes: Help me with onboarding
│  ○ No: Skip onboarding & don't ask again
```

**翻訳**
Storybookは初めて使いますか？
- Yes: Storybook初心者向けの案内（オンボーディング）を表示しますか？
  ▶ Storybookの基本構造（stories、Controls、Docs）を知りたい
- No: オンボーディングは不要。今後も聞かなくてOK
  ▶ 余計なファイルが増やしたくない/プロジェクト固有のルールがある
:::

---

:::details ターミナルのログを見る
```bash
$ npm create storybook@latest

┌  Initializing Storybook
│
●  Adding Storybook version 10.2.1 to your project
│
◇  Framework detected: nextjs-vite
│
◇  What configuration should we install?
│  Minimal: Just the essentials for component development.
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
│  - storybook@^10.2.1
│  - @storybook/nextjs-vite@^10.2.1
│  - vite@^7.3.1
│  - eslint-plugin-storybook@^10.2.1
│
◇  Dependencies installed
│
◇  Storybook was successfully installed in your project!
│
│  To run Storybook manually, run npm run storybook. CTRL+C to stop.
│
│  Wanna know more about Storybook? Check out https://storybook.js.org/
│  Having trouble or want to chat? Join us at https://discord.gg/storybook/
```
:::

`Storybook`のインストールが完了すると、次の画面が表示されます。

![installed-successfully-storybook](/images/books/learn-storybook-tutorial/installed-successfully-storybook.png)

