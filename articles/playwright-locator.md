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

|  role  |  tag  | 説明  |
| ---- | ---- | ---- |
|  form  |  <form>  |  ユーザーから入力を受け取るためのインターフェース  |
|  dialog  |  TD  |  TD  |

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





