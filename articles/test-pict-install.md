---
title: "[テスト] Windowsにpictを使えるようにする方法" # 記事のタイトル
emoji: '🧪' # アイキャッチとして使われる絵文字（1文字だけ）
type: 'tech' # tech: 技術記事 / idea: アイデア記事
topics: ['テスト', '初心者向け'] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開設定（falseにすると下書き）
---

## はじめに
この記事では、**Windows10上でGit bashでCLIで操作できる方法** をまとめております。
:::details 参考資料
@[card](https://gihyo.jp/magazine/SD/archive/2024/202402)
:::

## PICTとは？
> PICT (Pairwise Independent Combinatorial Testing) は、ソフトウェアテストにおいて「組み合わせテストケース」を効率的に生成するためのツールです。
> 特に、入力の全組み合わせを網羅するテストケースを作成するのは難しくコストがかかるため、PICT は「ペアワイズ法」を使って効率的に重要な組み合わせをカバーするテストケースを作成します。

@[card](https://github.com/microsoft/pict)



## 1. pict.exeをダウンロード
下記URLから最新のpict.exeをダウンロード
@[card](https://github.com/microsoft/pict/releases)

:::message
[note] 2024/11/10時点では、**Version 3.7.4**が最新でした。
:::
## 2. pict.exeをローカルの任意の場所に配置
自分は下記のファイルに配置しました。

```bash
c/Users/username/work/tools/pict/pict.exe
```

## 3. Git Bashでパスを通す
```bash:.bashrc
export PATH="$PATH:/c/Users/username/work/tools/pict"
```
:::details .bashrcが未作成の方は下記コマンドで作成
```bash
touch ~/.bashrc
```
:::

## 4. .bashrcを再読み込み
下記コマンドで`.bashrc`の再読み込み

```bash
source ~/.bashrc
```

## 5. pictコマンドの確認
下記コマンドでhelpをヘルプメッセージを表示
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
[info] 下記コマンドでpictコマンドが使えない方は、VSCode/PCの再起動を試してください
:::

:::message alert
[error] **msvcp140.dllが見つからないため、コードの実行を続行できません** or **コマンドを実行しても何も反応しない**と表示される方は下記を試してください
※自分はこれに該当しました。

  1. [Microsoft Visual C++ 2015 再頒布可能パッケージ](https://www.microsoft.com/ja-jp/download/details.aspx?id=53840)にアクセス
  2. ダウンロードをクリック
  3. vc_redist.x64.exe/vc_redist.x86.exeのいずれかを選択しダウンロード
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
