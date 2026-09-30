---
title: "環境構築"
---

## 🌱 この本について
`Storybook`は、UIコンポーネントをアプリ本体から切り離して、1つずつ表示・確認できるカタログツールです。
ボタンの色やサイズなどの表示パターンを一覧で確認でき、デザイナーやチームメンバーとの共有にも使えます。

本書では、[Storybook公式チュートリアル](https://storybook.js.org/tutorials/intro-to-storybook/react/ja/get-started/)を参考に、`Next.js` + `Tailwind CSS` + `TypeScript`の構成で`Storybook`を動かし、GitHub Pagesへの公開と、ブランチごとに別のURLで公開する方法までを扱います。
公式チュートリアルは`React` + `Vite`（ソースコードをブラウザで動く形に変換・配信するビルドツール）の構成のため、本書とは手順が異なります。

| チャプター | 内容 |
|---|---|
| 環境構築 | `Next.js`と`Storybook`をインストールする |
| 自作のUIコンポーネントを登録する | 自作のボタンを`Storybook`に表示する |
| GitHub Pagesにデプロイする | `Storybook`をGitHub Pagesに公開する |
| ブランチごとにGitHub Pagesを用意する | ブランチごとに別のURLで公開する |

**前提**
- `Node.js`（20.9以上）、`npm`、`Git`がインストールされていること
- GitHubアカウントを持っていること
- Reactのコンポーネントとprops、TypeScriptの型の基本が分かること

**動作確認したバージョン（執筆時点）**
- OS: Windows（ターミナルはPowerShell）
- Next.js: 16.1.6
- Storybook: 10.2.1

:::message
**ポイント**
`@latest`を指定してインストールすると、本書と異なるバージョンが入り、ログや生成されるファイルが本書と変わる場合があります。
本書と同じバージョンで試したい場合は、`npx create-next-app@16.1.6`、`npm create storybook@10.2.1`のようにバージョンを指定してください。
:::

## 🌱 このチャプターのゴール
ローカル環境で、下記のキャプチャーが表示されるところまで進めます。

![installed-successfully-storybook](/images/books/learn-storybook-tutorial/installed-successfully-storybook.png)

## 🌱 Next.jsのインストール
本書では`Next.js`上で`Storybook`を動かすため、まず`Next.js`を用意します。
作業用のフォルダを作成して移動し、ターミナルで次のコマンドを実行します。
フォルダ名は`package.json`のプロジェクト名にもなります（本書では`tech-storybook`）。

```bash
mkdir tech-storybook
cd tech-storybook
npx create-next-app@latest . --yes
```

- `.`: 今いるフォルダにプロジェクトを作成します
- `--yes`: 質問を省略し、既定の設定（`TypeScript`、`Tailwind CSS`、`ESLint`、App Routerなど）で作成します

初回は`Ok to proceed? (y)`とパッケージのインストールを確認されるため、`y`を入力して`Enter`を押します。

:::details ターミナルのログを見る
```text
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
```text
$ npm run dev

> tech-storybook@0.1.0 dev
> next dev

▲ Next.js 16.1.6 (Turbopack)
- Local:         http://localhost:3000

✓ Starting...
✓ Ready in 1375ms
 GET / 200 in 2.6s (compile: 2.2s, render: 331ms)
 GET / 200 in 117ms (compile: 12ms, render: 105ms)
```
:::

ブラウザで`http://localhost:3000`にアクセスし、下記の画面が表示されればOKです。
確認できたら、ターミナルで`Ctrl+C`を押して停止します。

![installed-successfully-nextjs](/images/books/learn-storybook-tutorial/installed-successfully-nextjs.png)


## 🌱 `app`ディレクトリを`src`ディレクトリ配下へ移動する

`Next.js`の`app`ディレクトリを`src`配下へ移動します。
`Next.js`としては必須ではなく、個人的な好みで行っています。
ただし、以降のチャプターは`src/app`の構成を前提に進めるため、この手順も実施してください。

PowerShellまたはGit Bashで、次のコマンドを実行します（コマンドプロンプトには`mv`がありません）。

```bash
mkdir src
mv app src/

```

```diff text
.
  ├── public
  ├── node_modules
- └── app
+ └── src
+     └── app

```

移動後に`npm run dev`を実行し、同じ画面が表示されることを確認します。
確認できたら、`Ctrl+C`で停止します。

:::message
**ポイント**
インポートエイリアス（`import x from "@/app/..."`のように、`@/`から始まるパスで読み込む書き方）を使う場合は、`tsconfig.json`の`paths`を`"@/*": ["./src/*"]`に変更してください。
本書のコードでは使っていないため、変更しなくても動作します。
:::

## 🌱 Storybookのインストール
続いて`Storybook`をインストールします。
```bash
npm create storybook@latest
```

実行すると、最初に`Storybook`を初めて使うかを聞かれます。
▶ 最小構成から必要なものを追加していくため、`No`を選択します

:::message
```text
◆  New to Storybook?
│  ● Yes: Help me with onboarding
│  ○ No: Skip onboarding & don't ask again
```

**翻訳**
Storybookは初めて使いますか？
- Yes: 初心者向けの案内（オンボーディング）を表示してほしい
- No: オンボーディングは不要。今後も聞かなくてOK
:::

`Yes`を選ぶと、次の構成の質問は表示されず、`Recommended`構成が自動で選ばれます。
以前に`No`を選んだことがある環境では、この質問は表示されません。

続いて、インストールする構成を聞かれます。
▶ `Minimal`を選択します

:::message
```text
◆  What configuration should we install?
│  ● Recommended: Component development, docs, and testing features.
│  ○ Minimal: Just the essentials for component development.
```

**翻訳**
どの構成をインストールしますか？
- Recommended: コンポーネント開発、ドキュメント、テストの機能を含む推奨構成
- Minimal: コンポーネント開発に必要な最小限の構成
:::

---

:::details ターミナルのログを見る
```text
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

`Framework detected: nextjs-vite`は、`Storybook`が`Next.js`用の構成（ビルドツールに`Vite`を使う構成）を自動で選んだことを示しています。
詳細は公式ドキュメントを参照してください。
@[card](https://storybook.js.org/docs/get-started/frameworks/nextjs-vite)

インストールが完了すると、`Storybook`が起動して次の画面が表示されます。
ブラウザが自動で開かない場合は、`npm run storybook`を実行して`http://localhost:6006`を開いてください。

![installed-successfully-storybook](/images/books/learn-storybook-tutorial/installed-successfully-storybook.png)

確認できたら、ターミナルで`Ctrl+C`を押して停止します。
