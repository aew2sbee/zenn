---
title: "[AWS] PowerShellでAWS Vaultコマンドを使えるようにする" # 記事のタイトル
emoji: "☁️" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア記事
topics: ["aws", "awsvault", "初心者向け", "powershell", "terraform"] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開:true / 非公開:false
---

## 🌱 はじめに

Terraform で AWS のサービスを管理するにあたり、チームでは AWS Vault で認証情報を管理していました。
そこで、Windows に AWS Vault をインストールした手順を、開発メンバーに共有するため執筆します。

| 項目 | 内容 |
| ---- | ---- |
| **対象者** | Windows で AWS Vault を使いたい方 |
| **伝えたい内容** | Scoop を使って AWS Vault をインストールし、バージョンを確認するまでの手順 |
| **前提条件** | Windows / PowerShell（管理者権限ではない通常の PowerShell） |

:::message
筆者の環境では、Git Bash では `aws-vault` コマンドがうまく動かず、PowerShell でのみ動作を確認できました。原因は未調査です。
:::

### AWS Vault とは

AWS Vault は、AWS のアクセスキーを `~/.aws/credentials` に平文で置かず、OS の資格情報ストア（Windows では資格情報マネージャー）に保管して使うためのツールです。
コマンドを実行するときは、保管した認証情報から一時的な認証情報を発行して渡します。

## 🌱 1. Scoop のインストール

今回は、Windows 用のパッケージマネージャーである `Scoop` を使って AWS Vault をインストールします。
パッケージマネージャーは、コマンドでソフトウェアをインストール・更新できるツールです。
（Chocolatey や、GitHub Releases の実行ファイルを直接ダウンロードする方法もあります）

:::message alert
Scoop のインストーラーは、管理者として実行した PowerShell では既定で中止されます。**管理者ではない通常の PowerShell** で実行してください。
:::

### 1-1. 実行ポリシーを変更する

Windows は既定で PowerShell スクリプトの実行を制限しています。
そこで、現在のユーザーに限り、ローカルのスクリプトと署名付きのリモートスクリプトの実行を許可します。

```powershell
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
```

下記のメッセージが表示された場合は、`Y` を入力して Enter を押してください（すでに設定済みの場合は表示されないことがあります）。

```text
Execution Policy Change
The execution policy helps protect you from scripts that you do not trust. Changing the execution policy might expose you to the security risks described in the
about_Execution_Policies help topic at https:/go.microsoft.com/fwlink/?LinkID=135170.
Do you want to change the execution policy?
[Y] Yes  [A] Yes to All  [N] No  [L] No to All  [S] Suspend  [?] Help (default is "N"): Y
```

### 1-2. Scoop をインストールする

Scoop 公式（[scoop.sh](https://scoop.sh)）のインストールスクリプトを取得して実行します。

```powershell
Invoke-RestMethod -Uri https://get.scoop.sh | Invoke-Expression
```

:::message
インターネットから取得したスクリプトをそのまま実行するコマンドです。信頼できる提供元のスクリプト以外は実行しないでください。
:::

インストール後、下記のコマンドでバージョンが表示されれば成功です。
`scoop` が見つからないと表示された場合は、PowerShell を開き直してください。

```powershell
scoop --version
```

## 🌱 2. AWS Vault のインストール

`Scoop` を使って AWS Vault をインストールします。

```powershell
scoop install aws-vault
```

`aws-vault --version` でバージョンを確認します。執筆時点では `v7.2.0` がインストールされました。

```text
PS C:\> aws-vault --version
v7.2.0
```

:::message
元の 99designs/aws-vault はメンテナンスが止まっており、現在の Scoop ではフォーク版（ByteNess/aws-vault）がインストールされます。そのため、表示されるバージョンは執筆時点と異なります。バージョン番号が表示されれば、インストールは成功です。
:::

## 🌱 おわりに

Scoop を使って AWS Vault をインストールし、PowerShell でバージョンを確認するところまでできました。
実際に使うには、`aws-vault add <プロファイル名>` でアクセスキーを登録し、`aws-vault exec <プロファイル名> -- terraform plan` のようにコマンドを実行します。

もっと楽に環境構築できる方法をご存じの方がいれば、教えていただきたいです。🙇‍♀️

## 🌱 参考

- [Scoop](https://scoop.sh)
- [ByteNess/aws-vault - GitHub](https://github.com/ByteNess/aws-vault)
- [99designs/aws-vault - GitHub](https://github.com/99designs/aws-vault)
