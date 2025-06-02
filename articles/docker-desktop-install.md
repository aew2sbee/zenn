---
title: '[Docker] Docker DeskTopのインストール' # 記事のタイトル
emoji: '🐳' # アイキャッチとして使われる絵文字（1文字だけ）
type: 'tech' # tech: 技術記事 / idea: アイデア記事
topics: ['python', 'django', '初心者向け'] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開設定（falseにすると下書き）
---

## はじめに

社内の有志メンバーの向けに Docker DeskTop のインストールの方法を解説します。

## Docker DeskTop のインストール

### 1. インストーラーのダウンロード

下記 URL をクリックし、Docker Desktop のインストーラーをダウンロードします。
@[card](https://docs.docker.com/desktop/install/windows-install/)

![docker_desktop_step0](/images/docker_desktop_step0.png)

### 2. インストール開始

デフォルトのままのチェック状態で`OK`をクリックしてください。
![docker_desktop_step1](/images/docker_desktop_step1.png)
インストールに少し時間がかかります。
![docker_desktop_step2](/images/docker_desktop_step2.png)

### 3. PC の再起動

:::message alert
`Close and restart`をクリックすると、 **PC が再起動します。注意してください！**

※WSL2 を有効化する為に、PC の再起動が必要です。
:::
![docker_desktop_step3](/images/docker_desktop_step3.png)
:::message alert
下記の画像が表示される場合は、**補足情報**を参照してください。
![docker_desktop_step5](/images/docker_desktop_step5.png)
:::

### 4. Docker Subscription Service Agreement を承認する

:::message
有料サブスクリプションサービスについて承認して利用します。
こちらの契約書は、主に企業やビジネス向けの契約書であり、個人利用にはあまり関係がありません。
:::
`Accept`をクリックしてください。

![docker_desktop_step4](/images/docker_desktop_step4.png)

### 5. インストール完了

下記の画像のように Docker Desktop が問題なく起動出来たら OK です！

![docker_desktop_step6](/images/docker_desktop_step6.png)

## Visual Studio Code の拡張機能の追加

### 1. Docker

![extension_docker](/images/extension_docker.png)

### 2. Dev Containers

![extension_dev_containers](/images/extension_dev_containers.png)

### 3. WSL

![extension_wsl](/images/extension_wsl.png)

## 補足情報

- **Q: WSL に関するエラー対応とは？** - A: 下記のようなエラーが発生する可能性があります。
  ![docker_desktop_step5](/images/docker_desktop_step5.png) 1. Linux カーネル更新するインストーラーをダウンロードする
  @[card](https://learn.microsoft.com/ja-jp/windows/wsl/install-manual#step-4---download-the-linux-kernel-update-package)
  ![docker_desktop_web](/images/docker_desktop_web.png) 2. インストーラーを実行する
  下記の画像のように手順を進め、WSL の更新を完了させてください。
  ![Linux_step0](/images/Linux_step0.png)
  ![Linux_step1](/images/Linux_step1.png)

## YouTube のご案内

ポモドーロタイマー（25 分勉強＋ 5 分休憩）を活用した作業・勉強配信を行っています。
集中したいときや、誰かと一緒に頑張りたいときに、ぜひご活用ください。

ご興味のある方は、ぜひお気軽に遊びに来てください！
「Zenn から来ました!!」とコメントを貰えると泣いて喜びます 🤣

@[card](https://www.youtube.com/@aew2sbee)
