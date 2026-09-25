---
title: "[Playwright] ロケーターの種類と使い方" # 記事のタイトル
emoji: "🎭" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア記事
topics: ["playwright", "初心者向け"] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開:true / 非公開:false
---

## 🌱 はじめに

この記事では、**Playwright のロケーター** についてまとめております。

@[card](https://gihyo.jp/book/2024/978-4-297-14220-9)

## 🌱 結論

:::message
下記の順番でロケーターを優先的に利用する

1. getByRole()
2. getByLabel()
3. getByPlaceholder()
4. getByText()
5. getByAltText()
6. getByTitle()

:::

:::message
上記は参考書籍に沿った順番です。Playwright の公式ドキュメントでは、getByRole → getByText → getByLabel → getByPlaceholder → getByAltText → getByTitle → getByTestId の順に紹介されています。
また、ロケーターの取得（`page.getByRole()` など）は同期的に Locator を返すため `await` は不要です。`await` は `click()` などの操作や `expect()` に付けます。
:::

## 🌱 1. getByRole()

> アクセシビリティ属性によって検索可能

```ts:基本的な使い方
const element = page.getByRole('role名');
```

```ts:サンプル
// ボタンを取得したい場合
const button = page.getByRole('button');
```

:::details 主要な role 属性の一覧

**コントロールに関する role とそれに対応する タグ**

```html:role属性="form"
<!-- ユーザーがデータを入力し、サーバーに送信するための領域（アクセシブルネームがある場合のみ form ロールになる） -->
<form>
```

```html:role属性="dialog"
<!-- モーダルウィンドウやダイアログボックスを表示するために使用 -->
<dialog>
```

```html:role属性="button"
<!-- ボタンを作成するためのタグ -->
<button>
<!-- <button>タグとは異なり、フォームデータの送信機能はありません。JavaScriptで動作をカスタマイズする際に使われます。 -->
<input type="button">
<!-- ボタンとして機能する画像を表示し、クリックするとフォームを送信できます。 -->
<input type="image">
<!-- フォーム内の入力フィールドをリセット（初期状態に戻す）するボタンを作成します。 -->
<input type="reset">
<!-- フォームの送信ボタンを作成します。クリックするとフォームデータが送信されます。 -->
<input type="submit">
```

```html:role属性="checkbox"
<!-- チェックボックスを作成します。複数の選択肢から複数の選択を可能にします。 -->
<input type="checkbox">
```

```html:role属性="spinbutton"
<!-- 数値の入力フィールドを作成します。 -->
<input type="number">
```

```html:role属性="radio"
<!-- ラジオボタンを作成します。複数の選択肢から1つだけ選択できる場合に使用されます。 -->
<input type="radio">
```

```html:role属性="slider"
<!-- スライダーを作成します。ユーザーが範囲内の数値を選択するために使用されます。 -->
<input type="range">
```

```html:role属性="textbox"
<!-- list 属性のない type=text/email/tel/url（type 省略を含む）の<input>。type=search は searchbox になる -->
<input type="text">
<!-- 複数行のテキスト入力フィールドを作成します。メモやコメントなど長文を入力する場合に使用されます。 -->
<textarea>
```

```html:role属性="combobox"
<!-- ドロップダウンリストを作成します。ユーザーがリストから1つの選択肢を選べるようにします（multiple 属性がなく、size が1以下の場合）。 -->
<select>
```

```html:role属性="listbox"
<!-- multiple 属性がある、または size が2以上の<select> -->
<select multiple>
```

```html:role属性="group"
<!-- <select>要素内で、選択肢をグループ化するために使用されます。リストの選択肢を整理する際に使われます。 -->
<optgroup>
```

```html:role属性="progressbar"
<!-- 進捗バーを表示するために使用されます。タスクの進行状況を視覚的に示すことができます。 -->
<progress>
```

**インライン要素に関する role とそれに対応する タグ**

```html:role属性="link"
<!-- ハイパーリンクを作成するためのタグです（href 属性がある場合のみ link ロールになる）。 -->
<a href>
<!-- 画像マップの中で、クリックできるエリアを定義するタグです（href 属性がある場合）。 -->
<area href>
```

```html:role属性="status"
<!-- ユーザーがフォームに入力したデータやスクリプトの結果を表示するためのタグです -->
<output>
```

**リストに関する role とそれに対応する タグ**

```html:role属性="list"
<!-- コマンドのリストを表すタグです（ARIA の menu ロールにはならない）。 -->
<menu>
<!-- 順序付きリストを作成するタグです -->
<ol>
<!-- 順序なしリストを作成するタグです。 -->
<ul>
```

```html:role属性="listitem"
<!-- リストの項目を定義するタグです。 -->
<li>
```

```html:role属性="definition"
<!-- 定義リスト内で、定義（説明）を記述するためのタグです。 -->
<dd>
```

```html:role属性="group"
<!-- 折りたたみ可能なインタラクティブな詳細情報の表示に使います。 -->
<details>
<!-- フォームの関連する入力フィールドやコントロールをグループ化するために使います。 -->
<fieldset>
```

```html:role属性="term"
<!-- その文脈で定義されている用語を示すタグです。 -->
<dfn>
<!-- 定義リスト（<dl>）内で、用語やアイテムを表すタグです。 -->
<dt>
```

**テーブルに関する role とそれに対応する タグ**

```html:role属性="table"
<!-- 表を作成するためのタグです。 -->
<table>
```

```html:role属性="caption"
<!-- テーブルのタイトルやキャプションを指定します。 -->
<caption>
```

```html:role属性="rowgroup"
<!-- テーブルのヘッダー部分を定義します。 -->
<thead>
<!-- テーブルの本体部分（行やデータセル）を定義します。 -->
<tbody>
<!-- テーブルのフッター部分を定義します。 -->
<tfoot>
```

```html:role属性="row"
<!-- HTMLテーブル内で行（row）を作成するためのタグです。 -->
<tr>
```

:::

## 🌱 2. getByLabel()

> 関連するラベルのテキストでフォームコントロールを検索可能
> label と input 要素が関連付けられている場合に便利

```ts:基本的な使い方
const label = page.getByLabel('ラベルの値');
```

```ts:サンプル
// <label for="searchbox">検索</label><input id="searchbox">に対してのコード
const label = page.getByLabel('検索');
```

## 🌱 3. getByPlaceholder()

> プレースホルダーをもとに入力欄を検索可能

```ts:基本的な使い方
const placeholder = page.getByPlaceholder('placeholderの値');
```

```ts:サンプル
// <input type="search" name="searchword" id="searchbox" placeholder="検索ワード">に対してのコード
const placeholder = page.getByPlaceholder('検索ワード');
```

## 🌱 4. getByText()

> テキストコンテンツで検索可能
> role を気にせず単純に目的のテキストを検索

```ts:基本的な使い方
const text = page.getByText('テキストの値');
```

```ts:サンプル
// <a href="/home">ホーム</a>に対してのコード
const text = page.getByText('ホーム');
```

## 🌱 5. getByAltText()

> 代替テキストによって要素(通常は画像)を検索可能

```ts:基本的な使い方
const img = page.getByAltText('alt属性の値');
```

```ts:サンプル
// <img alt="かわいいいぬ">に対してのコード
const img = page.getByAltText('かわいいいぬ');
```

## 🌱 6. getByTitle()

> タイトル属性によって要素を検索可能

```ts:基本的な使い方
const element = page.getByTitle('title属性の値');
```

```ts:サンプル
// <img alt="かわいいいぬ" title="2024/10/15 撮影">に対してのコード
const element = page.getByTitle('2024/10/15 撮影');
```
