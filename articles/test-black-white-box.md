---
title: "[テスト] ブラックボックステスト/ホワイトボックステストについて" # 記事のタイトル
emoji: '🧪' # アイキャッチとして使われる絵文字（1文字だけ）
type: 'tech' # tech: 技術記事 / idea: アイデア記事
topics: ['テスト', '初心者向け'] # タグ。["markdown", "rust", "aws"]のように指定する
published: false # 公開設定（falseにすると下書き）
---

## はじめに
この記事では、**Windows10上でGit bashでCLIで操作できる方法** をまとめております。
:::details 参考資料
@[card](https://gihyo.jp/magazine/SD/archive/2024/202402)
:::

## 1. ブラックボックステスト
:::message
- **ブラックボックステスト**: テスト対象をブラックボックスととらえて、その使用上の振る舞いを入出力に着目してテストケースを作成する技法
<!-- - **メリット**: リファクタリングに対応できる
- **デメリット**: ホワイトボックスより不具合の検出率が -->
:::

### 同値分割法
:::message
- **同値分割法**: テスト対象への入力を同等に処理されるグループに分けて、グループごとに任意の値をテストする技法
- **メリット**: 対象グループの1つの値をテストすれば十分なため、テスト数を削減できる
:::

> 例えば、「入力欄には1以上100以下の整数が入力できる」仕様があった場合 ⇒ **3パターン**
> - 0以下のグループ
> - 1以上100以下のグループ
> - 101以上のグループ

### 境界値分析
:::message
- **同値分割法**: テスト対象の仕様の中で「○○未満」「○○以上」など値に境界が存在する場合にその境界付近の値でテストする技法
- **メリット**: `>`や`>=`のイコールの有無の勘違いを検出することが可能
:::

> 例えば、「入力欄には1以上100以下の整数が入力できる」仕様があった場合 ⇒ **6パターン**
> - 0 (最小値の境界値 - 1)
> - 1 (最小値の境界値)
> - 2 (最小値の境界値 + 1)
> - 99 (最大値の境界値 - 1)
> - 100 (最大値の境界値)
> - 101 (最大値の境界値 + 1)

### ディジョンテーブルテスト(組み込みパターン表)
:::message
- **同値分割法**: 入力条件の組み合わせと対応する出力結果を整理っしてテストケースを作成する技法
- **メリット**: テストパターンを可視化することが出来、考慮漏れを防ぐことが出来る
:::

|    |    |  1. 条件記述部  |  2. 動作記述部  |  3. 条件指定部  |  4. 動作指定部  |
| ---- | ---- | ---- | ---- | ---- | ---- |
|  条件  |  タイムサービス時間内  |  T  |  T  |  T  |  F  |
|    |  割引対象商品  |  T  |  F  |  F  |  -  |
|    |  会員  |  -  |  T  |  F  |  -  |
|  動作  |  10%割引  |  X  |  -  |  -  |  -  |
|    |  5%割引  |  -  |  X  |  -  |  -  |
|    |  割引なし  |  -  |  -  |  X  |  X  |

※「T」:TRUE/ 「F」: FALSE
※「-」はその条件が動作に影響しない

@[card](https://www.veriserve.co.jp/helloqualityworld/service/gihoz/)
ディジョンテーブルテストをもとにテストケースを作成すると、、、

|  No  |  時間  |  商品  |  会員  |  期待結果  |
| ---- | ---- | ---- | ---- | ---- |
|  1  |  19時  |  衣類  |  -  |  10割引で商品で購入できる  |
|  2  |  19時  |  酒類  |  会員  | 5割引で商品で購入できる  |
|  3  |  19時  |  酒類  |  非会員  |  割引なしで購入できる  |
|  4  |  13時  |  -  |  -  |  割引なしで購入できる  |

※ 例えば、「18-20時がタイムサービス、酒類は割引対象害」仕様があった場合




### 状態遷移

## 2. ホワイトボックステスト











## PICTとは？
> PICT (Pairwise Independent Combinatorial Testing) は、ソフトウェアテストにおいて「組み合わせテストケース」を効率的に生成するためのツールです。
> 特に、入力の全組み合わせを網羅するテストケースを作成するのは難しくコストがかかるため、PICT は「ペアワイズ法」を使って効率的に重要な組み合わせをカバーするテストケースを作成します。

@[card](https://github.com/microsoft/pict)



## 1. pict.exeをダウンロード
下記URLから最新のpict.exeをダウンロード
@[card](https://github.com/microsoft/pict/releases)

![pict-install-step01](/images/articles/test-pict-install/pict-install-step01.png)

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
