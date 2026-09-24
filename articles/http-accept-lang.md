---
title: '[HTTP] Accept-Languageヘッダーでレスポンスの言語を切り替える' # 記事のタイトル
emoji: '🗣️' # アイキャッチとして使われる絵文字（1文字だけ）
type: 'tech' # tech: 技術記事 / idea: アイデア記事
topics: ['HTTPヘッダー', 'HTTP', '初心者向け'] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開:true / 非公開:false
---

## 🌱 はじめに

2023年ごろに下記の書籍を読みましたが、あまり深く理解できませんでした。
2025年に`情報セキュリティマネジメント試験`、`基本情報技術者試験`に合格し、基礎知識が身につきました。
改めて、書籍を読み直し、アウトプットを行い理解を深めていこうと思います。

https://www.shoeisha.co.jp/book/detail/9784798169477

## 🌱 結論

:::message

- クライアント（ブラウザなど）が送るリクエストヘッダーの`Accept-Language`を読み取り、返す言語を決める
- レスポンスヘッダーの`Content-Language`で、返したレスポンスの言語を示す

利用者の言語設定に合わせてメッセージを出し分けられる（多言語対応）というメリットがある

```diff ts
import { Router, Request, Response } from 'express';
const router = Router();

router.get('/', (req: Request, res: Response) => {
  let message = req.query.message as string | undefined;
  // リクエストヘッダーから「Accept-Language」の値を取得する
+  const acceptLanguage = req.headers['accept-language'] || '';
+  const lang = acceptLanguage.includes('en') ? 'en' : 'ja';

  // レスポンスヘッダーに「Content-Language」の値を設定する
+  res.setHeader('Content-Language', lang);
  res.send({ message });
});

```

:::

:::message alert
**本記事のコードの注意点**

- `Content-Language`は「このレスポンスが何語か」を示すだけで、CDNやプロキシのキャッシュを言語ごとに分ける働きはありません。キャッシュを分けるには、レスポンスに`Vary: Accept-Language`も付ける必要があります（Expressなら`res.vary('Accept-Language')`）。
- `acceptLanguage.includes('en')`は、優先度（`q`値）を考慮しない簡易的な判定です。日本語環境のブラウザは`ja,en-US;q=0.9,en;q=0.8`のように`en`を含む値を送ることがあり、その場合も英語が返ります。実際に使う場合は、Expressの`req.acceptsLanguages('ja', 'en')`のように優先度を考慮する方法を検討してください。
:::

参考: [Vary - HTTP | MDN](https://developer.mozilla.org/ja/docs/Web/HTTP/Headers/Vary)

## 🌱 検証

1. 言語設定が`英語`なら`英語`のメッセージを返却する
2. 言語設定が`日本語`なら`日本語`のメッセージを返却する

言語に応じたエラーメッセージの処理を追記する

```diff ts
import { Router, Request, Response } from 'express';
const router = Router();

router.get('/', (req: Request, res: Response) => {
  let message = req.query.message as string | undefined;
  const acceptLanguage = req.headers['accept-language'] || '';
  const lang = acceptLanguage.includes('en') ? 'en' : 'ja';

+  if (!message || message === '') {
+    res.status(400);
+    if (lang === 'en') {
+      message = 'The string is empty';
+    } else {
+      message = '文字列が空です';
+    }
+  }
  res.setHeader('Content-Language', lang);
  res.send({ message });
});
```

### 言語設定を`日本語`に設定して検証する

下記コマンドでAPIを実行させる

```bash
curl -s -H "Accept-Language: ja" -D - "http://localhost:3000/api?message="
```

:::details 実行結果を確認する

```bash
$ curl -s -H "Accept-Language: ja" -D - "http://localhost:3000/api?message="
HTTP/1.1 400 Bad Request
X-Powered-By: Express
X-timestamp: 2026-05-08T12:03:41.565Z
Content-Language: ja
Content-Type: application/json; charset=utf-8
Content-Length: 35
ETag: W/"23-AwQa53yr4bFJHPFwAWWf1y0U8W8"
Date: Fri, 08 May 2026 12:03:41 GMT
Connection: keep-alive
Keep-Alive: timeout=5

{"message":"文字列が空です"}
```

:::

### 言語設定を`英語`に設定して検証する

下記コマンドでAPIを実行させる

```bash
curl -s -H "Accept-Language: en" -D - "http://localhost:3000/api?message="
```

:::details 実行結果を確認する

```bash
$ curl -s -H "Accept-Language: en" -D - "http://localhost:3000/api?message="
HTTP/1.1 400 Bad Request
X-Powered-By: Express
X-timestamp: 2026-05-08T12:03:25.247Z
Content-Language: en
Content-Type: application/json; charset=utf-8
Content-Length: 33
ETag: W/"21-W7c1xzr/Out9Qs2GrAn3vmtzEi4"
Date: Fri, 08 May 2026 12:03:25 GMT
Connection: keep-alive
Keep-Alive: timeout=5

{"message":"The string is empty"}
```

:::

### `acceptLanguage`と`lang`の中身も確認する

ログを仕込み、サーバーを再起動する

```diff ts
import { Router, Request, Response } from 'express';
const router = Router();

router.get('/', (req: Request, res: Response) => {
  let message = req.query.message as string | undefined;
  const acceptLanguage = req.headers['accept-language'] || '';
  const lang = acceptLanguage.includes('en') ? 'en' : 'ja';
+  console.log(`Accept-Language: ${acceptLanguage}, lang: ${lang}`);

  // messageが空または未指定の場合、ステータス400（Bad Request）を返し、言語に応じたエラーメッセージを設定する
  if (!message || message === '') {
    res.status(400);
    if (lang === 'en') {
      message = 'The string is empty';
    } else {
      message = '文字列が空です';
    }
  }
  res.setHeader('Content-Language', lang);
  res.send({ message });
});
```

再び、APIを呼び出す

```bash
curl -s -H "Accept-Language: ja" -D - "http://localhost:3000/api?message="
```

:::details サーバー側のログを確認する

```text
$ npx tsx src/server.ts
Server is running on http://localhost:3000
Accept-Language: ja, lang: ja
```

:::

```bash
curl -s -H "Accept-Language: en" -D - "http://localhost:3000/api?message="
```

:::details サーバー側のログを確認する

```text
$ npx tsx src/server.ts
Server is running on http://localhost:3000
Accept-Language: en, lang: en
```

:::

## 🌱 検証レポジトリ

https://github.com/aew2sbee/poc-security