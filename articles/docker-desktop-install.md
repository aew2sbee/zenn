---
title: "[Docker] WindowsにDocker Desktopをインストールする" # 記事のタイトル
emoji: "🐳" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア記事
topics: ["docker", "wsl", "vscode", "初心者向け"] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開:true / 非公開:false
---

## 🌱 はじめに

Windows に Docker Desktop をインストールし、VS Code から Docker を使えるようにする方法を解説します。

Docker は、アプリとその実行環境をコンテナという箱にまとめ、どの PC でも同じ環境を再現できるツールです。
Docker Desktop は、Windows や Mac で Docker を GUI 付きで使うためのアプリです。Windows では、WSL2（Windows 上で Linux を動かす仕組み）を使って動作します。

:::message
スクリーンショットは執筆時点（2023年、Docker Desktop 4.18.0）のものです。現在のバージョンとは画面が異なる場合があるため、最新の手順は[公式ドキュメント](https://docs.docker.com/desktop/setup/install/windows-install/)も参照してください。
:::

### 動作要件

公式ドキュメントに記載されている主な要件は次のとおりです（2026年9月時点）。

- 64bit 版の Windows 10 22H2 以降、または Windows 11 23H2 以降
- WSL 2.1.5 以降
- RAM 8GB
- BIOS/UEFI でハードウェア仮想化が有効になっていること
- インストールに管理者権限が必要

## 🌱 Docker Desktop のインストール

### 1. インストーラーのダウンロード

下記の公式ページから、Docker Desktop のインストーラーをダウンロードします。
一般的な Windows PC（Intel/AMD の CPU）では、x86_64（AMD64）版を選択してください。

@[card](https://docs.docker.com/desktop/setup/install/windows-install/)

![Docker公式サイトのDocker Desktop for Windowsのダウンロードボタン](/images/articles/docker-desktop-install/docker_desktop_step0.png)

### 2. インストール開始

ダウンロードした `Docker Desktop Installer.exe` をダブルクリックして実行します。
設定画面では、`Use WSL 2 instead of Hyper-V`（WSL2 で Docker を動かす設定）にチェックが入っていることを確認し、`Ok` をクリックしてください。

![インストーラーの設定画面](/images/articles/docker-desktop-install/docker_desktop_step1.png)

インストールには少し時間がかかります。

![インストール中の画面](/images/articles/docker-desktop-install/docker_desktop_step2.png)

### 3. PC の再起動

:::message alert
`Close and restart` をクリックすると、**PC が再起動します。** 作業中のファイルは事前に保存してください。

WSL2 などの Windows の機能を新たに有効化した場合に、再起動が必要になります。
:::

![インストール完了後のClose and restartボタン](/images/articles/docker-desktop-install/docker_desktop_step3.png)

再起動後、Docker Desktop を起動してください。
WSL の更新を求めるエラーが表示された場合は、後述の「WSL のエラーが表示された場合」を参照してください。

### 4. Docker Subscription Service Agreement に同意する

Docker Desktop の利用規約への同意です。`Accept` をクリックしても、すぐに課金されるわけではありません。

![Docker Subscription Service Agreementの同意画面](/images/articles/docker-desktop-install/docker_desktop_step4.png)

:::message alert
個人利用、教育、非商用のオープンソース、小規模事業（従業員 250 人未満かつ年間売上 1,000 万米ドル未満）では無料で使えます。
それ以外の組織での業務利用や、政府機関での利用には有料サブスクリプションが必要です。会社で使う場合は、自社が対象かを確認してください。
:::

同意後にサインインやアンケートの画面が表示された場合は、スキップして進めても構いません。

### 5. インストール完了

下記の画像のように Docker Desktop が起動できたら OK です！

![Docker Desktopが起動した画面](/images/articles/docker-desktop-install/docker_desktop_step6.png)

PowerShell で下記のコマンドを実行し、`Hello from Docker!` と表示されれば、Docker が正しく動いています。

```powershell
docker run hello-world
```

## 🌱 WSL のエラーが表示された場合

Docker Desktop の起動時に、下記のような WSL のエラーが表示される場合があります。
WSL2 に必要な Linux カーネルが古い、またはインストールされていないために発生するエラーです。

![WSLの更新を求めるエラー画面](/images/articles/docker-desktop-install/docker_desktop_step5.png)

PowerShell を管理者として開き、下記のコマンドで WSL を更新してください（WSL 自体が入っていない場合は `wsl --install` を実行します）。

```powershell
wsl --update
wsl --version
```

`wsl --version` で WSL のバージョンが 2.1.5 以降になっていることを確認し、Docker Desktop を再起動します。

:::details wsl --update が使えない古い Windows の場合（執筆時点の手順）
執筆時点では、Linux カーネル更新プログラム パッケージを手動でインストールして解決しました。

1. Linux カーネル更新プログラム パッケージをダウンロードする

@[card](https://learn.microsoft.com/ja-jp/windows/wsl/install-manual#step-4---download-the-linux-kernel-update-package)

![Microsoftのドキュメントに掲載されているLinuxカーネル更新プログラムパッケージのリンク](/images/articles/docker-desktop-install/docker_desktop_web.png)

2. ダウンロードしたインストーラーを実行し、画面に従って WSL の更新を完了させる

![Linuxカーネル更新プログラムのセットアップ画面](/images/articles/docker-desktop-install/Linux_step0.png)

![Linuxカーネル更新プログラムのセットアップ完了画面](/images/articles/docker-desktop-install/Linux_step1.png)
:::

## 🌱 Visual Studio Code の拡張機能の追加

VS Code からコンテナを操作・開発するために、拡張機能を追加します。
VS Code をインストールしておき、拡張機能ビュー（`Ctrl+Shift+X`）で名前を検索して、提供元が Microsoft のものをインストールしてください。

### 1. Docker

コンテナやイメージの一覧を確認・操作できます。

![VS CodeのDocker拡張機能](/images/articles/docker-desktop-install/extension_docker.png)

### 2. Dev Containers

コンテナの中で開発できるようになります。

![VS CodeのDev Containers拡張機能](/images/articles/docker-desktop-install/extension_dev_containers.png)

### 3. WSL

WSL 上のファイルを VS Code で開けるようになります。

![VS CodeのWSL拡張機能](/images/articles/docker-desktop-install/extension_wsl.png)

## 🌱 おわりに

- インストーラーを実行し、WSL2 を使う設定のまま進める
- 会社で使う場合は、有料サブスクリプションが必要かを確認する
- WSL のエラーが出たら `wsl --update` で更新する
- VS Code に Docker・Dev Containers・WSL の拡張機能を入れる
