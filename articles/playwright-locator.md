---
title: "[Playwright] ロケーターの種類と使い方" # 記事のタイトル
emoji: '🐨' # アイキャッチとして使われる絵文字（1文字だけ）
type: 'tech' # tech: 技術記事 / idea: アイデア記事
topics: ['playwright', '初心者向け'] # タグ。["markdown", "rust", "aws"]のように指定する
published: false # 公開設定（falseにすると下書き）
---

## はじめに
この記事では、**Playwrightのロケーター** についてをまとめております。


## 結論
:::message
下記の順番でロケーターを優先的に利用する
1. getByRole()
2. getByLabel()
3. getByPlaceholder()
4. getByText()
5. getByAltText()
6. getByTitle()
:::


## 1. getByRole()
> アクセシビリティ属性によって検索可能

```ts: 基本的な使い方
const elements = await page.getByRole('role名');
```

```ts: サンプル
// ボタンを取得したい場合
const buttons = await page.getByRole('button');
```

:::details 主要なrole属性の一覧

### コントロールに関するroleとそれに対応するtag

```html:role属性="form"
<!-- ユーザーがデータを入力し、サーバーに送信するための領域 -->
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

```html:role属性="textboox"
上記以外の<input>
<!-- 複数行のテキスト入力フィールドを作成します。メモやコメントなど長文を入力する場合に使用されます。 -->
<textarea>
```

```html:role属性="combobox"
<!-- ドロップダウンリストを作成します。ユーザーがリストから1つまたは複数の選択肢を選べるようにします。 -->
<select>(mutiple属性がついておらず、sizeが1)
```

```html:role属性="listbox"
<select>(上記以外)
```

```html:role属性="group"
<!-- <select>要素内で、選択肢をグループ化するために使用されます。リストの選択肢を整理する際に使われます。 -->
<optgroup>
```

```html:role属性="progressbar"
<!-- 進捗バーを表示するために使用されます。タスクの進行状況を視覚的に示すことができます。 -->
<progress>
```

### インライン要素に関するroleとそれに対応するtag

```html:role属性="link"
<!-- ハイパーリンクを作成するためのタグです。 -->
<a>
<!-- 画像マップの中で、クリックできるエリアを定義するタグです。 -->
<area>
```

```html:role属性="status"
<!-- ユーザーがフォームに入力したデータやスクリプトの結果を表示するためのタグです -->
<output>
```

### リストに関するroleとそれに対応するtag

```html:role属性="list"
<!-- メニューを作成するためのタグです。 -->
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

<details>
<fieldset>
```

```html:role属性="term"

<dfn>
<dt>
```

### テーブルに関するroleとそれに対応するtag

```html:role属性="table"

<table>
```

```html:role属性="caption"

<caption>
```

```html:role属性="rowgroup"

<tbody>
<tfoot>
<thead>
```

```html:role属性="row"

<tr>
```

```html:role属性="cell"
通常の<table>に属しscope属性のない<th>,<td>
```

```html:role属性="gridecll"

```

```html:role属性="columnheadr"

```

```html:role属性="rowheader"

```

:::



## 2. getByLabel()
> 関連するラベルのテキストでフォームコントロールを検索可能
> label と input 要素が関連付けられている場合に便利

```ts: 基本的な使い方
const label = await page.getByLabel('ラベルの値');
```

```ts: サンプル
// <label for="searcbox">検索</label>に対してのコード
const label = await page.getByLabel('/検索/');
```

## 3. getByPlaceholder()
> プレースホルダーをもとに入力欄を検索可能


```ts: 基本的な使い方
const placeholder = await page.getByPlaceholder('placeholderの値');
```

```ts: サンプル
// <input type="search" name="searchword" id="searchbox" placeholder="検索ワード">に対してのコード
const placeholder = await page.getByPlaceholder('/検索ワード/');
```

## 4. getByText()
> テキストコンテンツで検索可能
> roleを気にせず単純に目的のテキストを検索

```ts: 基本的な使い方
const text = await page.getByText('テキストの値');
```

```ts: サンプル
// <a herf="/home">ホーム</a>に対してのコード
const text = await page.getByText('/ホーム/');
```

## 5. getByAltText()
> 代替テキストによって要素(通常は画像)を検索可能

```ts: 基本的な使い方
const img = await page.getByAltText('alt属性の値');
```

```ts: サンプル
// <img alt="かわいいいぬ">に対してのコード
const img = await page.getByAltText('/かわいいいぬ/');
```

## 6. getByTitle()
> タイトル属性によって要素を検索可能

```ts: 基本的な使い方
const img = await page.getByAltText('title属性の値');
```

```ts: サンプル
// <img alt="かわいいいぬ" title="2024/10/15 撮影">に対してのコード
const img = await page.getByAltText('/2024/10/15 撮影/');
```





