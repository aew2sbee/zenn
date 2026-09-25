---
title: '[CORS] アクセス許可するオリジンを限定する方法' # 記事のタイトル
emoji: '🔒' # アイキャッチとして使われる絵文字（1文字だけ）
type: 'tech' # tech: 技術記事 / idea: アイデア記事
topics: ['cors', 'express', 'typescript', 'security', '初心者向け'] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開:true / 非公開:false
---

## 🌱 はじめに

2023年ごろに下記の書籍を読みましたが、あまり深く理解できませんでした。
2025年に`情報セキュリティマネジメント試験`、`基本情報技術者試験`に合格し、基礎知識が身につきました。
改めて、書籍を読み直し、アウトプットを行い理解を深めていこうと思います。

https://www.shoeisha.co.jp/book/detail/9784798169477

## 🌱 結論

:::message

1. `ALLOW_ORIGIN_LIST`に許可するオリジンを定義する
2. `req.headers.origin`でリクエスト元のオリジンを取得する
3. `res.header('Access-Control-Allow-Origin', origin);`で、許可されたオリジンのときだけレスポンスヘッダーにセットする
4. オリジンによってレスポンスが変わるため、`res.vary('Origin');`で`Vary: Origin`を付ける
5. プリフライトリクエスト（`OPTIONS`）では、`Access-Control-Allow-Headers`で許可するヘッダーを指定する

`Access-Control-Allow-Origin: *`のようにワイルドカードで全オリジンを許可せず、許可リストで制限する。
これにより、**意図しないオリジンのWebページから、ブラウザ経由でレスポンスを読み取られることを防げる**。

```diff ts
-import { Router, Request, Response } from 'express';
+import { Router, Request, Response, NextFunction } from 'express';
 const router = Router();

+const ALLOW_ORIGIN_LIST = [
+  'http://localhost:3000',
+];

-router.use((req: Request, res: Response, next: Function) => {
+router.use((req: Request, res: Response, next: NextFunction) => {
+  // Originによってレスポンスが変わることをキャッシュに伝える
+  res.vary('Origin');
+
+  const origin = req.headers.origin;
+  if (origin && ALLOW_ORIGIN_LIST.includes(origin)) {
+    res.header('Access-Control-Allow-Origin', origin);
+  }
+
+  if (req.method === 'OPTIONS') {
+    // X-Tokenヘッダーを許可する
+    res.header('Access-Control-Allow-Headers', 'X-Token');
+    res.sendStatus(204);
+    return;
+  }
   next();
 });
```

:::

:::message alert
CORSは、ブラウザが「別オリジンのレスポンスをJavaScriptに渡してよいか」を判断する仕組みです。サーバーへのアクセス自体を遮断するものではありません。
`curl`のようなブラウザ以外のクライアントは、許可リストにないオリジンからでもリクエストを送り、レスポンスを受け取れます。APIを保護するには、認証・認可が別途必要です。
:::

## 🌱 検証

本記事では、APIサーバーを`http://localhost:8080`で起動し、`http://localhost:3000`で動くフロントエンドからのアクセスを許可する想定で検証します。

:::details 検証に使ったサーバーのコード（server.ts）

```ts:server.ts
import express from 'express';
import router from './router';

const app = express();
app.use('/api', router);

app.listen(8080, () => {
  console.log('API server: http://localhost:8080');
});
```

`router.ts`の末尾には、動作確認用に下記を追加しています。

```ts:router.ts
router.get('/', (req: Request, res: Response) => {
  res.json({ message: 'ok' });
});

export default router;
```

:::

まず、`ALLOW_ORIGIN_LIST`にCORSを許可するオリジンを定義する。

```diff ts:router.ts
 import { Router, Request, Response, NextFunction } from 'express';
 const router = Router();

+const ALLOW_ORIGIN_LIST = [
+  'http://localhost:3000',
+];

 router.use((req: Request, res: Response, next: NextFunction) => {
   next();
 });
```

次に、リクエストのオリジン（`req.headers.origin`）を取得し、`ALLOW_ORIGIN_LIST`に定義されているか確認する。
許可するオリジンは1つに限らないため、リクエストに合わせて`Access-Control-Allow-Origin`の値を変える。
このとき、キャッシュ（CDNなど）が別のオリジン向けのレスポンスを使い回さないように、`Vary: Origin`を付ける。

```diff ts:router.ts
 import { Router, Request, Response, NextFunction } from 'express';
 const router = Router();

 const ALLOW_ORIGIN_LIST = [
   'http://localhost:3000',
 ];

 router.use((req: Request, res: Response, next: NextFunction) => {
+  // Originによってレスポンスが変わることをキャッシュに伝える
+  res.vary('Origin');
+
+  const origin = req.headers.origin;
+  if (origin && ALLOW_ORIGIN_LIST.includes(origin)) {
+    res.header('Access-Control-Allow-Origin', origin);
+  }
+
   next();
 });
```

最後に、プリフライトリクエスト（`OPTIONS`）に対して`X-Token`ヘッダーを許可する処理を追加する。
プリフライトリクエストは本リクエストの前の確認なので、ルートの処理には進めず、`204 No Content`で応答を終える。

```diff ts:router.ts
 import { Router, Request, Response, NextFunction } from 'express';
 const router = Router();

 const ALLOW_ORIGIN_LIST = [
   'http://localhost:3000',
 ];

 router.use((req: Request, res: Response, next: NextFunction) => {
   // Originによってレスポンスが変わることをキャッシュに伝える
   res.vary('Origin');

   const origin = req.headers.origin;
   if (origin && ALLOW_ORIGIN_LIST.includes(origin)) {
     res.header('Access-Control-Allow-Origin', origin);
   }

+  if (req.method === 'OPTIONS') {
+    // X-Tokenヘッダーを許可する
+    res.header('Access-Control-Allow-Headers', 'X-Token');
+    res.sendStatus(204);
+    return;
+  }
   next();
 });
```

:::message
今回の本リクエストは`GET`のため、`Access-Control-Allow-Methods`は不要です。
`PUT`や`DELETE`などを許可する場合は、`res.header('Access-Control-Allow-Methods', 'GET, PUT, DELETE');`のように指定します。
:::

下記のコマンドで、APIにプリフライトリクエストを送信して検証する。

まず、許可リストにない`http://dev.localhost:3000`から、`X-Token`ヘッダー付きの`GET`リクエストが許可されないことを確認する。

```bash
curl -s -X OPTIONS \
  -H "Origin: http://dev.localhost:3000" \
  -H "Access-Control-Request-Method: GET" \
  -H "Access-Control-Request-Headers: X-Token" \
  -D - "http://localhost:8080/api"
```

`204`は返るが、レスポンスヘッダーに`Access-Control-Allow-Origin`が含まれていない。
そのため、ブラウザはプリフライトが失敗したと判断し、`X-Token`ヘッダー付きの`GET`リクエストを送信しない。

:::details 実行結果を確認する

```shell
$ curl -s -X OPTIONS \
  -H "Origin: http://dev.localhost:3000" \
  -H "Access-Control-Request-Method: GET" \
  -H "Access-Control-Request-Headers: X-Token" \
  -D - "http://localhost:8080/api"
HTTP/1.1 204 No Content
X-Powered-By: Express
Vary: Origin
Access-Control-Allow-Headers: X-Token
ETag: W/"a-bAsFyilMr4Ra1hIU5PyoyFRunpI"
Date: Thu, 24 Sep 2026 12:08:58 GMT
Connection: keep-alive
Keep-Alive: timeout=5
```

:::

次に、許可リストにある`http://localhost:3000`から、`X-Token`ヘッダー付きの`GET`リクエストが許可されることを確認する。

```bash
curl -s -X OPTIONS \
  -H "Origin: http://localhost:3000" \
  -H "Access-Control-Request-Method: GET" \
  -H "Access-Control-Request-Headers: X-Token" \
  -D - "http://localhost:8080/api"
```

レスポンスヘッダーに`Access-Control-Allow-Origin: http://localhost:3000`が含まれているので、オリジンとして許可されていることが確認できた。

:::details 実行結果を確認する

```shell
$ curl -s -X OPTIONS \
  -H "Origin: http://localhost:3000" \
  -H "Access-Control-Request-Method: GET" \
  -H "Access-Control-Request-Headers: X-Token" \
  -D - "http://localhost:8080/api"
HTTP/1.1 204 No Content
X-Powered-By: Express
Vary: Origin
Access-Control-Allow-Origin: http://localhost:3000
Access-Control-Allow-Headers: X-Token
ETag: W/"a-bAsFyilMr4Ra1hIU5PyoyFRunpI"
Date: Thu, 24 Sep 2026 12:08:58 GMT
Connection: keep-alive
Keep-Alive: timeout=5
```

:::
