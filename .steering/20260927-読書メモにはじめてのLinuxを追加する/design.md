<!-- AI向け: 見出しは追加しない。事実のみを書き、所感・補足は書かない。requirements.md の要件をどう実現するかを書く。 -->

# 設計

## 🌱 方針
<!-- AI向け: 実現方法の全体像を1〜3文で書く。 -->
『Software Design for Beginners② はじめてのLinux』のチャプターを、1冊につき1ブランチ・1PRで追加する。書籍情報と本の概要を先に追加し、読む目的と読書メモを書いてからレビューを受ける。

## 🌱 作業の流れ
<!-- AI向け: 1単位の作業を番号付きリストで順に書く。 -->
1. `docs/book064-linux-beginners` ブランチを作成する
2. book064.md に書籍情報と本の概要を書き、config.yaml に book064 を追加する
3. 下書きPRを作成する
4. 読む目的と読書メモを書く
5. サブエージェントでレビューする
6. レビュー指摘を修正してコミットする
7. `npx zenn preview` で表示を確認する
8. PRをレビュー待ちにする

## 🌱 詳細
<!-- AI向け: 流れの各ステップで必要なルール（命名規則・判断基準・使うツールなど）を書く。形式は自由。 -->

### ブランチ
- ブランチ名: `docs/book064-linux-beginners`
- ベースブランチ: `docs/book-record-of-reading-notes`（#164）。#164 のマージ後に main に変更する

### チャプター
- ファイル: `books/book-record-of-reading/book064.md`
- タイトル: `2026.09: Software Design for Beginners② はじめてのLinux`
- 見出しの順: 書籍情報 → 本の概要 → 読む目的 → 読書メモ → 作成した記事（記事がある場合のみ）
- 書籍情報のURL: https://gihyo.jp/book/2026/978-4-297-15755-5

### config.yaml
- chapters の `book059`（読書中）の直後に、読了年月の新しい順になるよう book064 を追加する
- 同時に追加する book060〜book064 のPRはすべて同じ位置を変更するため、後からマージするPRでは並び順（book064 → book063 → book062 → book061 → book060）を保って競合を解消する
