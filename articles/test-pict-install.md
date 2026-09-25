---
title: "[テスト] Windowsでpictを使えるようにする方法" # 記事のタイトル
emoji: "🧪" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア記事
topics: ["テスト", "pict", "初心者向け"] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開:true / 非公開:false
---

## 🌱 はじめに

この記事では、**Windows10 上で PICT を使えるようにする方法** をまとめています。

@[card](https://gihyo.jp/magazine/SD/archive/2024/202402)

### 前提条件

- Git Bash（Git for Windows）がインストールされていること
- 以降のコマンドは、すべて Git Bash で実行します

## 🌱 PICT とは？

PICT (Pairwise Independent Combinatorial Testing) は、Microsoft が公開している、組み合わせテストのテストケースを生成するツールです。
入力の全組み合わせをテストするのはコストがかかるため、PICT は「ペアワイズ法」を使います。任意の 2 つのパラメーターの値の組み合わせをすべて 1 回以上含む、少ない数のテストケースを生成します。

@[card](https://github.com/microsoft/pict)

## 🌱 1. pict.exe をダウンロード

下記 URL から最新の pict.exe をダウンロードします。
@[card](https://github.com/microsoft/pict/releases)

![pict-install-step01](/images/articles/test-pict-install/pict-install-step01.png)

:::message
2024/11/10 時点では、**Version 3.7.4**（2022/03/31 公開）が最新でした。2026/09 時点でも、最新のリリースは同じ Version 3.7.4 です。
:::

## 🌱 2. pict.exe をローカルの任意の場所に配置

自分は下記のパスに配置しました。`username`は、ご自身のユーザー名に置き換えてください。

```text
C:\Users\username\work\tools\pict\pict.exe
```

Git Bash では、このパスを`/c/Users/username/work/tools/pict/pict.exe`と表記します。

## 🌱 3. Git Bash でパスを通す

`~/.bashrc`（`C:\Users\username\.bashrc`）の末尾に、下記の 1 行を追記します。

```bash:.bashrc
export PATH="$PATH:/c/Users/username/work/tools/pict"
```

:::details .bashrc が未作成の方は下記コマンドで作成

```bash
touch ~/.bashrc
```

:::

エディタを使わずに追記する場合は、下記コマンドを実行します。

```bash
echo 'export PATH="$PATH:/c/Users/username/work/tools/pict"' >> ~/.bashrc
```

## 🌱 4. .bashrc を再読み込み

下記コマンドで`.bashrc`を再読み込みします。

```bash
source ~/.bashrc
```

## 🌱 5. pict コマンドの確認

下記コマンドでヘルプメッセージを表示します。

```bash
pict /?
```

:::details 実行結果を確認する

```bash
$ pict /?
Pairwise Independent Combinatorial Testing

Usage: pict model [options]

Options:
 /o:N|max - Order of combinations (default: 2)
 /d:C     - Separator for values  (default: ,)
 /a:C     - Separator for aliases (default: |)
 /n:C     - Negative value prefix (default: ~)
 /e:file  - File with seeding rows
 /r[:N]   - Randomize generation, N - seed
 /c       - Case-sensitive model evaluation
 /s       - Show model statistics

```

:::

:::message
上記コマンドで pict コマンドが使えない方は、Git Bash（VSCode のターミナルを使っている場合は VSCode）を開き直すか、PC を再起動してください。
:::

:::message alert
「**msvcp140.dll が見つからないため、コードの実行を続行できません**」と表示される、または**コマンドを実行しても何も反応しない**方は、下記を試してください。
※自分はこれに該当しました。

1. [サポートされている最新の Visual C++ 再頒布可能パッケージ](https://learn.microsoft.com/ja-jp/cpp/windows/latest-supported-vc-redist)にアクセス
2. 64bit 版の pict.exe なら`X64`、32bit 版なら`X86`のインストーラー（vc_redist.x64.exe / vc_redist.x86.exe）をダウンロード（どちらか分からない場合は、両方インストールしても問題ありません）
3. インストーラーを実行し、インストール

※ 執筆時は「Microsoft Visual C++ 2015 再頒布可能パッケージ」をインストールしましたが、2015 版はサポートが終了しているため、最新版のリンクに差し替えています。
:::

## 🌱 6. 動作確認

任意の作業フォルダで、下記コマンドを実行して`test.txt`を作成します。

```bash
touch test.txt
```

作成した`test.txt`に、下記内容を記述します。

```text:test.txt
DATA1: A, B, C
DATA2: 1, 2, 3
```

下記コマンドを実行します。

```bash
pict test.txt
```

:::details 実行結果を確認する

```bash
$ pict test.txt
DATA1   DATA2
C       1
C       3
A       1
A       2
B       1
C       2
A       3
B       3
B       2
```

:::
