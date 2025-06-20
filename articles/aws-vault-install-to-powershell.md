---
title: '[AWS] PowerShellにAWS Vaultコマンドを使えるようにする' # 記事のタイトル
emoji: '☁️' # アイキャッチとして使われる絵文字（1文字だけ）
type: 'tech' # tech: 技術記事 / idea: アイデア記事
topics: ['aws', 'awsvault', '初心者向け', 'powerShell', 'terraform'] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開設定（falseにすると下書き）
---

## はじめに

Terraform で AWS のサービスを管理したく、その際に`AWS Vaultコマンド`が必要だったので、
開発メンバーに共有するため執筆します。

本当は Git bash で操作を行いたかったのですが、`AWS Vaultコマンド`が反応せず、
`powerShell`のみしか出来なかったです。
:::message
※Terraform が使えるようになるために色々作業しました。
もっと楽に環境構築出来る気がします。ご存知の方が教えて頂きたいです。🙇‍♀️
:::
| 項目 | 内容 |
| ---- | ---- |
| **前提条件** | Windows |

## 1. Scoop のインストール

まず、`AWS Vault`コマンドが使えるようにするには、`Scoop`が必要です。
`PowerShell`を下記コマンドを実行して、`Scoop`をインストールします

```bash
Set-ExecutionPolicy RemoteSigned -scope CurrentUser
invoke-Expression (New-Object System.Net.WebClient).DownloadString('https://get.scoop.sh')
```

ポリシー変更のために以下のメッセージが表示されるので Y を押してください。

```bash
Execution Policy Change
The execution policy helps protect you from scripts that you do not trust. Changing the execution policy might expose you to the security risks described in the
about_Execution_Policies help topic at https:/go.microsoft.com/fwlink/?LinkID=135170.
Do you want to change the execution policy?
[Y] Yes  [A] Yes to All  [N] No  [L] No to All  [S] Suspend  [?] Help (default is "N"): Y
```

## 2. AWS Vault のインストール

`Scoop`を使って`AWS Vault`をインストールします

```bash
scoop install aws-vault
```

`version`を確認します

```bash
aws-vault --version
```

`version`は、`v7.2.0`である事が確認出来ました。

```bash
$ aws-vault --version
v7.2.0
```

## おわりに

もっと楽に環境構築出来る気がします。ご存知の方が教えて頂きたいです。🙇‍♀️
MacOS ならもっと楽ですよね。。

## YouTube のご案内

ポモドーロタイマー（25 分勉強＋ 5 分休憩）を活用した作業・勉強配信を行っています。
集中したいときや、誰かと一緒に頑張りたいときに、ぜひご活用ください。

ご興味のある方は、ぜひお気軽に遊びに来てください！
「Zenn から来ました!!」とコメントを貰えると泣いて喜びます 🤣

@[card](https://www.youtube.com/@aew2sbee)
