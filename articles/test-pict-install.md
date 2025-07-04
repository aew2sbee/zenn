---
title: '[テスト] Windowsにpictを使えるようにする方法' # 記事のタイトル
emoji: '🧪' # アイキャッチとして使われる絵文字（1文字だけ）
type: 'tech' # tech: 技術記事 / idea: アイデア記事
topics: ['テスト', 'pict', '初心者向け'] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開設定（falseにすると下書き）
---

## はじめに

この記事では、**Windows10 上で Git bash で CLI で操作できる方法** をまとめております。
:::details 参考資料
@[card](https://gihyo.jp/magazine/SD/archive/2024/202402)
:::

## PICT とは？

> PICT (Pairwise Independent Combinatorial Testing) は、ソフトウェアテストにおいて「組み合わせテストケース」を効率的に生成するためのツールです。
> 特に、入力の全組み合わせを網羅するテストケースを作成するのは難しくコストがかかるため、PICT は「ペアワイズ法」を使って効率的に重要な組み合わせをカバーするテストケースを作成します。

@[card](https://github.com/microsoft/pict)

## 1. pict.exe をダウンロード

下記 URL から最新の pict.exe をダウンロード
@[card](https://github.com/microsoft/pict/releases)

![pict-install-step01](/images/articles/test-pict-install/pict-install-step01.png)

:::message
2024/11/10 時点では、**Version 3.7.4**が最新でした。
:::

## 2. pict.exe をローカルの任意の場所に配置

自分は下記のファイルに配置しました。

```bash
c/Users/username/work/tools/pict/pict.exe
```

## 3. Git Bash でパスを通す

```bash:.bashrc
export PATH="$PATH:/c/Users/username/work/tools/pict"
```

:::details .bashrc が未作成の方は下記コマンドで作成

```bash
touch ~/.bashrc
```

:::

## 4. .bashrc を再読み込み

下記コマンドで`.bashrc`の再読み込み

```bash
source ~/.bashrc
```

## 5. pict コマンドの確認

下記コマンドで help をヘルプメッセージを表示

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
下記コマンドで pict コマンドが使えない方は、VSCode/PC の再起動を試してください
:::

:::message alert
**msvcp140.dll が見つからないため、コードの実行を続行できません** or **コマンドを実行しても何も反応しない**と表示される方は下記を試してください
※自分はこれに該当しました。

1. [Microsoft Visual C++ 2015 再頒布可能パッケージ](https://www.microsoft.com/ja-jp/download/details.aspx?id=53840)にアクセス
2. ダウンロードをクリック
3. vc_redist.x64.exe/vc_redist.x86.exe のいずれかを選択しダウンロード
4. インストーラーを実行/インストール

:::

## 6. 動作確認

下記コマンドで`test.txt`を作成

```bash
touch test.txt
```

作成した`test.txt`に下記内容を編集

```text: test.txt
DATA1: A, B, C
DATA2: 1, 2, 3
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

## YouTube のご案内

ポモドーロタイマー（25 分勉強＋ 5 分休憩）を活用した作業・勉強配信を行っています。
集中したいときや、誰かと一緒に頑張りたいときに、ぜひご活用ください。

ご興味のある方は、ぜひお気軽に遊びに来てください！
「Zenn から来ました!!」とコメントを貰えると泣いて喜びます 🤣

@[card](https://www.youtube.com/@aew2sbee)
