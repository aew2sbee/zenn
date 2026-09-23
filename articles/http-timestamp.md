---
title: '[API] HTTPヘッダーにサーバーの現在時刻を付与する' # 記事のタイトル
emoji: '⌚' # アイキャッチとして使われる絵文字（1文字だけ）
type: 'tech' # tech: 技術記事 / idea: アイデア記事
topics: ['HTTPヘッダー', 'HTTP', '初心者向け'] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開:true / 非公開:false
---

## 🌱 はじめに

2023年ごろに下記の書籍を読みましたが、あまり深く理解できませんでした。
2025年に`情報セキュリティマネジメント試験`、`基本情報技術者試験`に合格し、基礎知識が身につきました。
改めて書籍を読み直し、アウトプットを通して理解を深めていこうと思います。

https://www.shoeisha.co.jp/book/detail/9784798169477

## 🌱 結論

:::message
HTTPのレスポンスヘッダーに、独自ヘッダー`Timestamp`としてサーバーの現在時刻を付与する

**クライアントがサーバーとの時刻のずれを把握し、認証トークンの有効期限が切れる前に更新するかを判断する、といった用途に活用できます。**
ただし、有効期限の最終的な判定はサーバー側で行います。

```diff ts
 import { Router, Request, Response } from 'express';
 const router = Router();

 router.get('/', (req: Request, res: Response) => {
+  // レスポンスヘッダーに現在の日時（ISO形式）を「Timestamp」として付与する
+  res.setHeader('Timestamp', new Date().toISOString());
   let message = req.query.message as string | undefined;

   res.send({ message });
 });
```

:::

:::message alert
**標準の`Date`ヘッダーとの違い**

HTTPには、サーバーがレスポンスを生成した日時を示す標準の`Date`ヘッダーがあり、Expressでも自動で付与されます。
ただし`Date`は秒単位で、形式も`Thu, 07 May 2026 12:35:11 GMT`のようなHTTPの日付形式です。
ミリ秒単位の時刻や、ISO 8601形式（`2026-05-07T12:35:11.115Z`）で扱いたい場合に、独自ヘッダーを追加します。

なお、別オリジンのフロントエンドから`fetch`でこのヘッダーを読む場合は、`Access-Control-Expose-Headers: Timestamp`の設定も必要です。
:::

参考: [Date - HTTP | MDN](https://developer.mozilla.org/ja/docs/Web/HTTP/Headers/Date)

## 🌱 HTTPヘッダーに`Timestamp`を付与する前

HTTPヘッダーに`Timestamp`を**付与していない**コードを用意します。

```ts
import { Router, Request, Response } from 'express';
const router = Router();

router.get('/', (req: Request, res: Response) => {
  let message = req.query.message as string | undefined;

  res.send({ message });
});
```

ブラウザの検証ツール（DevTools）で「Network」タブを開いた状態でAPIを実行し、レスポンスヘッダーの中身を確認します。
赤枠の位置に`Timestamp`は存在しません。標準の`Date`ヘッダーには秒単位のサーバー時刻が含まれています。
なお、この画像はブラウザのキャッシュが使われたため、ステータスが`304 Not Modified`になっています。

![before](/images/articles/http-timestamp/before.png)

## 🌱 HTTPヘッダーに`Timestamp`を付与した後

HTTPヘッダーに`Timestamp`を**付与した**コードを用意します。

```ts
import { Router, Request, Response } from 'express';
const router = Router();

router.get('/', (req: Request, res: Response) => {
  // レスポンスヘッダーに現在の日時（ISO形式）を「Timestamp」として付与する
  res.setHeader('Timestamp', new Date().toISOString());
  let message = req.query.message as string | undefined;

  res.send({ message });
});
```

同様に、検証ツールで「Network」タブを開いた状態でAPIを実行し、レスポンスヘッダーの中身を確認します。
赤枠の部分に`Timestamp`が存在し、ミリ秒単位のサーバー時刻を取得できました。

![after](/images/articles/http-timestamp/after.png)

## 🌱 検証レポジトリ

https://github.com/aew2sbee/poc-security
