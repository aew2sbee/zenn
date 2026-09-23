---
title: "[GitHub] Your main branch isn't protected に対応する" # 記事のタイトル
emoji: "🐙" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア記事
topics: ["github", "git", "初心者向け"] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開:true / 非公開:false
---

## 🌱 はじめに

リポジトリのトップページを開いたときに、下記のような警告が表示されることがあります。

> Your main branch isn't protected
> Protect this branch from force pushing or deletion, or require status checks before merging.

![step1-warning-banner](/images/articles/github-branch-protection-ruleset/step1-warning-banner.png)

これは「**main ブランチに何の保護も設定されていない**」という GitHub からのお知らせです。
この記事では、`Rulesets`（ルールセット）を使って main ブランチを保護する手順を解説します。

:::message
**前提条件**

- リポジトリのオーナー、または admin 権限を持っていること（`Settings` タブの表示に必要）
- パブリックリポジトリ、または GitHub Pro（個人）/ Team / Enterprise Cloud プランのリポジトリであること（Free プランのプライベートリポジトリでは、ルールセットを作成しても強制されません）

画面は執筆時点のものです。GitHub の画面は変更されることがあります。
:::

:::details 参考資料
@[card](https://docs.github.com/ja/repositories/configuring-branches-and-merges-in-your-repository/managing-rulesets/about-rulesets)
:::

## 🌱 なぜ保護が必要なのか

保護が設定されていない main ブランチでは、書き込み権限を持つ人なら誰でも、下記の操作を実行できてしまいます。

:::message

- **force push（強制プッシュ）**: `git push --force`で他の人のコミットを消してしまう
- **ブランチの削除**: main ブランチそのものを消してしまう
- **レビューや CI を通さない変更**: レビューを経ずに、または CI が失敗したままマージ・push してしまう

:::

どれも一度やってしまうと復旧に手間がかかるため、事前にルールで防いでおきます。

## 🌱 結論

:::message
`Settings` > `Rulesets` から**ブランチルールセットを新規作成**し、
`Enforcement status` を `Active` にし、対象ブランチに**デフォルトブランチ**を指定して、下記 2 つにチェックを入れる。

- `Restrict deletions`: ブランチの削除を禁止する
- `Block force pushes`: force push を禁止する

:::

## 🌱 1. Rulesets の設定画面を開く

リポジトリの`Settings`タブを開き、左メニューの
`Code, planning, and automation` > `Rulesets` を選択します。

![step2-settings-rulesets](/images/articles/github-branch-protection-ruleset/step2-settings-rulesets.png)

:::message
警告バナーの`Protect this branch`ボタンを押すと、この設定画面へ直接移動できます。
:::

:::message alert
**プライベートリポジトリでは、プランによってはルールセットが適用されません。**
上記の画像のように

> Your rulesets won't be enforced on this private repository until you move to GitHub Team organization account.

と表示される場合、ルールセットを作成しても実際には強制されません。
ルールセットを強制するには、パブリックリポジトリにするか、GitHub Pro（個人）/ Team / Enterprise Cloud のプランを利用する必要があります。
:::

## 🌱 2. New branch ruleset を選択する

`New ruleset`ボタンを押して、`New branch ruleset`を選択します。

![step3-new-branch-ruleset](/images/articles/github-branch-protection-ruleset/step3-new-branch-ruleset.png)

:::message

- **New branch ruleset**: ブランチに対するルール（今回はこちら）
- **New tag ruleset**: タグに対するルール
- **Import a ruleset**: JSON ファイルから設定を読み込む

:::

## 🌱 3. ルールセット名と適用状態を設定する

`Ruleset Name`に分かりやすい名前を入力し、`Enforcement status`を`Active`にします。

![step4-ruleset-name-enforcement](/images/articles/github-branch-protection-ruleset/step4-ruleset-name-enforcement.png)

| 項目 | 設定値 | 説明 |
|------|--------|------|
| **Ruleset Name** | `main-protection` | ルールセットの名前（任意） |
| **Enforcement status** | `Active` | ルールを有効にする |

:::message
`Enforcement status`には下記の状態があります。

- `Active`: ルールを適用する
- `Disabled`: ルールを適用しない（一時停止）
- `Evaluate`: 違反を記録するだけで、実際にはブロックしない（お試し用。GitHub Enterprise のみ）

:::

なお、`Bypass list` は、ルールの対象外にしたい人やアプリを登録する項目です。
今回は誰も例外にしないため、空のままにします（空でも、リポジトリの管理者はルールセット自体を編集・無効化できます）。

## 🌱 4. 対象のブランチを指定する

`Target branches`の`Add target`から`Include default branch`を選択します。

![step5-target-branches](/images/articles/github-branch-protection-ruleset/step5-target-branches.png)

| 選択肢 | 説明 |
|--------|------|
| **Include default branch** | デフォルトブランチ（main など）を対象にする |
| **Include all branches** | すべてのブランチを対象にする |
| **Include by pattern** | `release/*`のようなパターンで対象を指定する |
| **Exclude by pattern** | パターンに一致するブランチを対象から除外する |

:::message
`Include default branch`を選んでおくと、デフォルトブランチの名前を
`main`から変更した場合でも、設定を直す必要がありません。
:::

## 🌱 5. 適用するルールを選択する

`Branch rules`から、適用したいルールにチェックを入れます。
今回は`Restrict deletions`と`Block force pushes`の 2 つを有効にします。

上記は主なルールの抜粋です。画面には、ほかにもコードスキャンなどに関するルールが表示されます。

![step6-branch-rules](/images/articles/github-branch-protection-ruleset/step6-branch-rules.png)

| ルール | 説明 |
|--------|------|
| **Restrict creations** | ブランチの新規作成を制限する |
| **Restrict updates** | ブランチの更新（push）を制限する |
| **Restrict deletions** | ブランチの削除を禁止する |
| **Require linear history** | マージコミットの push を禁止する（PR では squash / rebase マージのみ可能になる） |
| **Require deployments to succeed** | 指定環境へのデプロイ成功を必須にする |
| **Require signed commits** | 署名済みコミットのみ許可する |
| **Require a pull request before merging** | PR を経由しない変更（直接 push など）を禁止する |
| **Require status checks to pass** | CI などのチェック成功を必須にする |
| **Block force pushes** | force push を禁止する |

:::message
`Restrict deletions` と `Block force pushes` の 2 つは、誤操作による事故を防げます。1 人で使っているリポジトリでも、最初に入れておくのがおすすめです。
:::

:::message alert
`Require a pull request before merging`を有効にすると、
**main ブランチへ直接 push できなくなります。**
1 人で運用している場合は、作業フローが変わる点に注意してください。
:::

## 🌱 6. Create で作成する

ページ下部の`Create`ボタンを押して、ルールセットを作成します。

![step7-create](/images/articles/github-branch-protection-ruleset/step7-create.png)

作成が完了すると、Rulesets の一覧にルールセットが `Active` として表示されます。
リポジトリのトップページを再読み込みすると、`Your main branch isn't protected` の警告が消えていることを確認できます。

## 🌱 動作確認

ルールセットが適用されている状態で main ブランチに force push を試すと、
下記のように GitHub 側で拒否されます。

:::message alert
ローカルとリモートに差分がないと `Everything up-to-date` と表示されるだけで、動作を確認できません。
また、ルールが効いていない状態で差分のある force push をすると、main の履歴が実際に書き換わります。
動作確認は、テスト用のリポジトリで行うことをおすすめします。
:::

```text
$ git push --force origin main
remote: error: GH013: Repository rule violations found for refs/heads/main.
remote:
remote: - Cannot force-push to this branch
```

## 🌱 おわりに

`Rulesets`は設定項目が多く、最初は身構えてしまいますが、
`Restrict deletions`と`Block force pushes`の 2 つだけでも
「うっかり main を壊す」事故はかなり防げます。

まずは最小限の設定から始めて、チームの運用に合わせて
`Require a pull request before merging`や`Require status checks to pass`を
追加していくのが良いと思います。
