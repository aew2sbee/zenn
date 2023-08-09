---
title: "【Docker】Docker DeskTopのインストール" # 記事のタイトル
emoji: "🧜‍♀️" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア記事
topics: ["python", "django", "初心者向け"] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開設定（falseにすると下書き）
---
## はじめに
社内の有志メンバーの向けにDocker DeskTopのインストールの方法を解説します。

## Docker DeskTopのインストール
### 1. インストーラーのダウンロード
下記URLをクリックし、Docker Desktopのインストーラーをダウンロードします。
@[card](https://docs.docker.com/desktop/install/windows-install/)

![docker_desktop_step0](/images/docker_desktop_step0.png)

### 2. インストール開始
デフォルトのままのチェック状態で`OK`をクリックしてください。
![docker_desktop_step1](/images/docker_desktop_step1.png)
インストールに少し時間がかかります。
![docker_desktop_step2](/images/docker_desktop_step2.png)

### 3. PCの再起動
:::message alert
`Close and restart`をクリックすると、 **PCが再起動します。注意してください！**

※WSL2を有効化する為に、PCの再起動が必要です。
:::
![docker_desktop_step3](/images/docker_desktop_step3.png)
:::message alert
下記の画像が表示される場合は、**補足情報**を参照してください。
![docker_desktop_step5](/images/docker_desktop_step5.png)
:::


### 4. Docker Subscription Service Agreementを承認する
:::message
有料サブスクリプションサービスについて承認して利用します。
こちらの契約書は、主に企業やビジネス向けの契約書であり、個人利用にはあまり関係がありません。
:::
`Accept`をクリックしてください。

![docker_desktop_step4](/images/docker_desktop_step4.png)

### 5. インストール完了
下記の画像のようにDocker Desktopが問題なく起動出来たらOKです！

![docker_desktop_step6](/images/docker_desktop_step6.png)

## Visual Studio Codeの拡張機能の追加
### 1. Docker
![extension_wsl](/images/extension_wsl.png)
### 2. Dev Containers
![extension_dev_containers](/images/extension_dev_containers.png)
### 3. WSL
![extension_docker](/images/extension_docker.png)

## おわりに
Docker DeskTopで環境差分をなくしましょう！