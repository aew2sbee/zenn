---
title: '[Playwright] 0から勉強して導入した話' # 記事のタイトル
emoji: '🎭' # アイキャッチとして使われる絵文字（1文字だけ）
type: 'tech' # tech: 技術記事 / idea: アイデア記事
topics: ['playwright', '初心者向け'] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開設定（falseにすると下書き）
---

## はじめに

この記事では、社内 PJ に**Playwright を導入した体験談** についてをまとめております。

## 結論

:::message
Playwright の学習コストは高かったが、テームで **テストの関心** や **自動化テストの実用性** を実感することが出来た
:::

## 導入背景

なぜ、Playwright を導入するようになったのか?
新規のプログラムに対しては、以前からバックエンド側でユニットテストを実施しておりました。
しかし、既存のプログラムは一部フロントエンド側で計算処理を行っていたため、満足するユニットテストを実施する事が出来なかった。

ですが、Playwright の導入により、
既存のプログラムに対して、**フロントエンド側/バックエンド側の計算処理のテストを実施する**ことが可能になるから

## 良かった点

### 1. 各環境のテスト工数の軽減

現 PJ のリリースまでのテストフローは下記の通りでした。

1. ローカル環境でテストを行う
2. DEV 環境でテストを行う
3. STG 環境でテストを行う
4. PRD 環境でテストを行う

最低でも 4 回同じ内容のテストを以前までは、手動手動確認で行っておりましたが、(丸一日手動テストする日もありました)
Playwright(自動化テスト)のおかげで 2~4 まではテストの実行結果を見守るだけになりました。

### 2. デグレに対するストレス軽減

Playwright をコーディング最中に`プログラムミス`や`意図しない挙動`を発覚することがありました。
以前は、手動確認だったので 1 から手動確認を行っており苦労しました。
しかし、これまで作成した Playwright を再度実行するだけで良くなりました。

## 苦労した点

### 1. HTML のエレメント要素がぐちゃぐちゃ

既存のプログラムは、綺麗に表示されていれば問題ない HTML になっていました。
そのため、目的のデータを取得したいのに label 等がなく苦肉の策でデータを取得する Playwright になってしまった。

:::message alert
苦肉の策でデータを取得する Playwright=仕様変更やプログラム変更に対して脆いコード
:::

### 2. ロード時間

各機能やページに対してロードを示すデータが異なっていたため、それらを内包する関数の作成が難しかった
また、ロード待機関数をちゃんと作成してもロード中のデータを取得しようとし`undefined`でテストが失敗するケースが散見されました。

## 工夫した点

### 1. ラッパー関数を作成

Playwright の既存関数を使用しても何も問題はないのですが、行数が増えたり、可読性が低下によって、
Playwright が他メンバーに受け入れにくいを避けるためにシンプルなテスト攻勢を意識しました。

```diff tsx: サンプル
- await page.goto('https://www.google.com');
+ await openPage(page, 'https://www.google.com');
```

### 2. sleep 時間をむやみに使わない

ページが完全に表示されるまで、多めの sleep 時間を設けることでロード時間の対処することはできます。
しかし、テスト件数が何百件の場合、**テスト件数 \* 多めの sleep 時間 = 何時間**　という単位になります。

なので、sleep 時間を極力使用せず、下記コードのように活用しました。

```tsx
// ネットワークがアイドル状態になるのを待つ
await page.waitForLoadState('networkidle');
```

## YouTube のご案内

ポモドーロタイマー（25 分勉強＋ 5 分休憩）を活用した作業・勉強配信を行っています。
集中したいときや、誰かと一緒に頑張りたいときに、ぜひご活用ください。

ご興味のある方は、ぜひお気軽に遊びに来てください！
「Zenn から来ました!!」とコメントを貰えると泣いて喜びます 🤣

@[card](https://www.youtube.com/@aew2sbee)
