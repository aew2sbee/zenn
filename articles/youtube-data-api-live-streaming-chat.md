---
title: '[YouTube API] ライブ/配信中のチャットを取得する' # 記事のタイトル
emoji: '🎥' # アイキャッチとして使われる絵文字（1文字だけ）
type: 'tech' # tech: 技術記事 / idea: アイデア
topics: ['python', 'youtubeapi', 'youtubelive', 'youtubedataapi', '初心者向け'] # タグ。["markdown", "rust", "aws"]のように指定する
published: false # 公開設定（falseにすると下書き）
---

## はじめに

この記事では、**Browser Use**を触る機会がありその感想をまとめます。

:::details 参考資料
@[card](https://zenn.dev/gunjo/articles/8450e69537dbb6)
:::
@[card](https://gihyo.jp/magazine/SD)

## 結論


:::message alert

:::


## 0. 前提条件
```txt: requirements.txt
google-api-python-client
python-dotenv
```

:::details 詳細情報
```bash

$ pip show google-api-python-client
Name: google-api-python-client
Version: 2.160.0
Summary: Google API Client Library for Python
Home-page: https://github.com/googleapis/google-api-python-client/
Author: Google LLC
Author-email: googleapis-packages@google.com
License: Apache 2.0
Location: C:\Users\xxxxx\AppData\Local\Programs\Python\Python312\Lib\site-packages
Requires: google-api-core, google-auth, google-auth-httplib2, httplib2, uritemplate
```
```bash
$ pip show python-dotenv
Name: python-dotenv
Version: 1.0.1
Summary: Read key-value pairs from a .env file and set them as environment variables
Home-page: https://github.com/theskumar/python-dotenv
Author: Saurabh Kumar
Author-email: me+github@saurabh-kumar.com
License: BSD-3-Clause
Location: C:\Users\xxxxx\AppData\Local\Programs\Python\Python312\Lib\site-packages
Requires:
Required-by: browser-use, lmnr
```
:::

## 1. インストール


https://www.youtube.com/watch?v=FM_T0rNH-Bg


```bash
pip install -r requirements.txt
```

## 2. コーディング

```.env
YOUTUBE_API_KEY=XXXXXXXXXXXXXXXXXXXXXXX
VIDEO_ID=XXXXXXXXXX
```


```py
import os
from googleapiclient.discovery import build
from datetime import datetime, timezone
from dotenv import load_dotenv

# .envファイルの読み込み
load_dotenv()

# .envから環境変数を取得
API_KEY = os.getenv('YOUTUBE_API_KEY')  # APIキー
VIDEO_ID = os.getenv('VIDEO_ID')  # 動画ID

# YouTube APIのクライアントのセットアップ
YOUTUBE_API_SERVICE_NAME = "youtube"
YOUTUBE_API_VERSION = "v3"
youtube = build(YOUTUBE_API_SERVICE_NAME, YOUTUBE_API_VERSION, developerKey=API_KEY)

def get_live_chat_id(api_key, video_id):
    """ 指定したビデオIDのライブチャットIDを取得する """
    youtube = build("youtube", "v3", developerKey=api_key)

    response = youtube.videos().list(
        part="liveStreamingDetails",
        id=video_id
    ).execute()

    items = response.get("items", [])
    if not items:
        print("ライブ動画が見つかりませんでした")
        return None

    live_chat_id = items[0]["liveStreamingDetails"].get("activeLiveChatId")
    if not live_chat_id:
        print("ライブチャットIDが見つかりませんでした")
        return None

    return live_chat_id

def get_live_chat_messages(api_key, live_chat_id):
    """ 指定したライブチャットIDのメッセージを取得する """
    youtube = build("youtube", "v3", developerKey=api_key)

    response = youtube.liveChatMessages().list(
        liveChatId=live_chat_id,
        part="snippet,authorDetails"
    ).execute()

    messages = []
    for item in response.get("items", []):
        author = item["authorDetails"]["displayName"]
        message = item["snippet"]["displayMessage"]
        timestamp = item["snippet"]["publishedAt"]

        # タイムスタンプを UTC から日本時間 (JST) に変換
        dt = datetime.fromisoformat(timestamp.replace("Z", "+00:00"))
        jst = dt.astimezone(timezone(timedelta(hours=9)))  # JST に変換

        messages.append(f"[{jst.strftime('%Y-%m-%d %H:%M:%S')}] {author}: {message}")

    return messages

if __name__ == "__main__":
    live_chat_id = get_live_chat_id(API_KEY, VIDEO_ID)
    if live_chat_id:
        messages = get_live_chat_messages(API_KEY, live_chat_id)
        for msg in messages:
            print(msg)

```

## 3. 結果

```bash
python src/main.py
```
```bash
$ python src/main.py
[2025-01-31 02:14:43] 小倉あん: テスト
[2025-01-31 02:14:46] 小倉あん: テスト
[2025-01-31 02:14:48] 小倉あん: テスト
```


ちゃんと最新の記事のタイトルを取得することは出来ました！
```json
{"done":{"text":"The latest article by 小倉さん on Zenn is '[ドキュメント] わかりやすい文章を書くには', published 14 days ago."}}
```


https://www.youtube.com/watch?v=HOtk_cMPP8o&list=PL6PuwuVHc-IBACCr4Pu0qFFDo4Ax67pKq

https://console.cloud.google.com/

![youtube-api-queries](/images/articles/youtube-data-api-live-streaming-chat/youtube-api.png)
![youtube-api-queries](/images/articles/youtube-data-api-live-streaming-chat/youtube-api-queries.png)