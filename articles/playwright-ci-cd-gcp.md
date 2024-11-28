---
title: "[Playwright] CI/CDで社内環境に対してGitHub ActionsのIPを許可する in GCP編" # 記事のタイトル
emoji: "🎭‍" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア記事
topics: ["playwright", "github", "cicd", "googlecloud", "contest2024"] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開設定（falseにすると下書き）
---


## はじめに
この記事では、**社内で初めてCI/CDに組み込む時に苦労した点と対策**を解説します。
:::details 実際に使ったyaml
```yaml
name: Playwright Tests

on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

jobs:
  e2e-tests:
    runs-on: ubuntu-latest
    permissions:
      id-token: write
      contents: read
    steps:
    - uses: actions/checkout@v4
    - uses: google-github-actions/auth@v2
      with:
        workload_identity_provider: 'projects/123456789/locations/global/workloadIdentityPools/my-pool/providers/my-provider'
        service_account: 'example@project.iam.gserviceaccount.com'

    # GitHub ActionsのIPは可変のため、GitHub Actionsのアクセスを許可するルールをWAFに追加する
    - name: Allow Security-Policy
      run: |
        IP=$(curl -f -s http://whatismyip.akamai.com)
        echo "The IP address is: $IP"
        gcloud compute security-policies rules create NUMBER --action allow --security-policy XXXXX --description "GitHub Actions IP" --src-ip-ranges ${IP}

    - uses: actions/setup-node@v4
      with:
        node-version: 18
        cache: npm
        cache-dependency-path: package-lock.json
    - name: Install dependencies
      run: npm ci

    - name: Install Playwright Browsers
      run: npx playwright install --with-deps

    # WAFのルールが適応に時間が必要なため待機する
    - name: Wait for WAF to apply (up to 3 minutes)
      run: |
        sleep 90
        for i in {1..3}; do
          if curl -f https://example.com; then
            echo "Success"
            break
          fi
          echo "Retrying in 30 seconds..."
          sleep 30
        done

    # テスト実行
    - name: Run Playwright tests
      run: npx playwright test

    # 先ほど追加したWAFの許可ルールを削除する
    - name: Delete Security-Policy
      if: ${{ always() }}
      run: |
        gcloud compute security-policies rules delete NUMBER --security-policy XXXXX --quiet
```
:::

## 1. google-github-actions/authのエラーが解消できない
このエラーを解消するのに大変時間を要しました。
:::message alert
`GitHub Action`で実行時のエラー
```bash
google-github-actions/auth failed with: gitHub Actions did not inject $ACTIONS_ID_TOKEN_REQUEST_TOKEN or $ACTIONS_ID_TOKEN_REQUEST_URL into this job. This most likely means the GitHub Actions workflow permissions are incorrect, or this job is being run from a fork. For more information, please see https://docs.github.com/en/actions/security-guides/automatic-token-authentication#permissions-for-the-github_token
```
:::
上記のエラーを解消する時に試したことを列挙します。
お困りの方は、ご参考になれば幸いです。

### 1. `id-token`の書き込み権限を許可する
当時 `id-token`の書き込み権限を許可しても上記のエラーは解消しませんでした。
```diff yaml
jobs:
  e2e-tests:
    runs-on: ubuntu-latest
+    # Add "id-token" with the intended permissions.
+    permissions:
+      id-token: write
+      contents: read
    steps:
```

@[card](https://github.com/google-github-actions/auth?tab=readme-ov-file#usage)



### 2. `google-github-actions/auth`の書き方が正しいか
下記項目について確認しました。
- `google-github-actions/auth`のver
- `workload_identity_provider`の値
- `service_account`の値
```yaml
- uses: google-github-actions/auth@v2
  with:
    workload_identity_provider: 'projects/123456789/locations/global/workloadIdentityPools/my-pool/providers/my-provider'
    service_account: 'example@project.iam.gserviceaccount.com'
```

@[card](https://github.com/google-github-actions/auth?tab=readme-ov-file#usage)



### 3. 対象レポジトリーの`Settings`が適切か
下記項目にチェックが付いているのかを確認しました。

- [x] **Fork pull request workflows** > Run workflows from fork pull requests
- [x] **Workflow permissions** > Read and write permissions

![gihub-setting](/images/articles/playwright-ci-cd-gcp/gihub-setting.png)


:::message

**下記項目は組織アカウントで許可されていませんでした。**

---

**Send write tokens to workflows from fork pull requests.**
This tells Actions to send tokens with write permissions to workflows from pull requests originating from repository forks. Note that doing so will give maintainers of those forks write permissions against the source repository.
> フォークされたリポジトリからのプルリクエストに対して、書き込み権限のトークンを送信する。

> これにより、Actions はフォーク元リポジトリからのプルリクエストに対して書き込み権限を持つトークンをワークフローに送信します。なお、この設定を有効にすると、フォークの管理者が元のリポジトリに対して書き込み権限を持つことになります。

---

**Send secrets and variables to workflows from fork pull requests.**
This tells Actions to send repository secrets and variables to workflows from pull requests originating from repository forks.

> フォークされたリポジトリからのプルリクエストに対して、シークレットと変数を送信する。

> これにより、Actions はフォーク元リポジトリからのプルリクエストに対してリポジトリのシークレットと変数をワークフローに送信します。

---

**Choose whether GitHub Actions can create pull requests or submit approving pull request reviews.**
Allow GitHub Actions to create and approve pull requests

> GitHub Actions にプルリクエストを作成したり、プルリクエストを承認するレビューを送信させるかどうかを選択する。

> GitHub Actions がプルリクエストを作成および承認できるように許可します。
---
:::

### 4. `service_account`をCLIから作成する
当初はGUIから`service_account`を作成しておりましたが、`CLI`で実行すると必要な設定ができるみたいです。
下記URLが参考サイトみたいです。

@[card](https://docs.github.com/ja/actions/using-github-hosted-runners/using-github-hosted-runners/about-github-hosted-runners)

※インフラ担当の方に依頼したので詳細はよく分かりません。

### 5. `google-github-actions/auth`の前に`actions/checkout`を追加する
下記サイトを参考に`actions/checkout`を追加しましたが、解消には繋がりませんでした。
@[card](https://github.com/google-github-actions/auth?tab=readme-ov-file#prerequisites)

```diff yaml
+ - uses: actions/checkout@v4
  - uses: google-github-actions/auth@v2
    with:
      workload_identity_provider: 'projects/123456789/locations/global/workloadIdentityPools/my-pool/providers/my-provider'
      service_account: 'example@project.iam.gserviceaccount.com'
```


### 6. Fork先から実行するのではなく、Fork元から実行する
この対応で`google-github-actions/auth`のエラーが解消されました。
:::message
Fork元で新しいブランチを作成し、新しくPRを作成したら、解消されました!
:::


## 2. WAFが有効になるのに時間がかかる
当たり前の話かもしれませんが、WAFは即座反映されません。
そのため、WAF設定が有効になるのを待つ必要があります。

方法はいくつかあるとおもいますが、自分は下記コードをyamlに追加しました。
※ 大体1-3分の間で有効になることを観測しております。
```yaml
# WAFのルールが適応に時間が必要なため待機する
- name: Wait for WAF to apply (up to 3 minutes)
  run: |
    sleep 90
    for i in {1..3}; do
      if curl -f https://example.com; then
        echo "Success"
        break
      fi
      echo "Retrying in 30 seconds..."
      sleep 30
    done

```
## 3. JobごとでGithub ActionsのIPが変わる
当初は各Jobを管理しやすいように細かく分けましたが、各JobでIPが変わります。
そのため、`allow-security-policy`で許可したIP以外で`Playwright`が実行されて`403 Forbidden`が表示されて困りました。
しかし、下記サイトを読み各Jobを1つにまとめ`403 Forbidden`を解消しました。

@[card](https://zenn.dev/hsaki/articles/github-actions-component)

下記のように`yaml`を変更しました。

```diff yaml
jobs:
-  allow-security-policy:
-    runs-on: ubuntu-latest
-    steps:
-    # --- 以下略 ---
-
-  run-playwright-tests
-    runs-on: ubuntu-latest
-    steps:
-    # --- 以下略 ---
-
-  delete-security-policy
-    runs-on: ubuntu-latest
-    steps:
-    # --- 以下略 ---

+  e2e-tests:
+    runs-on: ubuntu-latest
+    steps:
+    # --- 以下略 ---
```


## 4. 海外アクセスブロックよりも優先度を高くする
既存のWAFのルールに国内以外からのアクセスは拒否のルールがありました。
Githubは海外になるため、上記のルールよりも優先度を高くする必要があります。

## 5. Playwrightのために追加したWAFのルールは必ず削除する
試行錯誤中は最後まで処理を進まないケースがあります。
その時、前回のWAFのルールが残ったまま下記のエラーが発生しておりました。
※ そのたびにインフラ担当の方に削除連絡をしておりました。

:::message alert
```bash
ERROR: (gcloud.compute.security-policies.rules.create) Could not fetch resource:
 - Invalid value for field 'resource.priority': 'NUMBER'. Cannot have rules with the same priorities.
```
:::

`if: ${{ always() }}`を追加し**テストの失敗/キャンセル/処理の失敗**でも必ず削除処理を実行する必要があります。

```diff yaml
    # 先ほど追加したWAFの許可ルールを削除する
    - name: Delete Security-Policy
+      if: ${{ always() }}
      run: |
        gcloud compute security-policies rules delete NUMBER --security-policy XXXXX --quiet
```


## 6. 連続でCI/CDはできない
WAFの優先度を固定の数値にしているため、`priority`の重複エラーが発生します。
再度確認したい場合は、**前回の処理が終わるのを待つ or キャンセルする**必要があります。

:::message alert
```bash
ERROR: (gcloud.compute.security-policies.rules.create) Could not fetch resource:
 - Invalid value for field 'resource.priority': 'NUMBER'. Cannot have rules with the same priorities.
```
:::
