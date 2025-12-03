---
title: "[Browser Use] 触ってみた結果と感想" # 記事のタイトル
emoji: "🧠" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["ai", "python", "browseruse", "playwright", "初心者向け"] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開設定（falseにすると下書き）
---

## はじめに

この記事では、**Browser Use**を触る機会がありその感想をまとめます。

:::details 参考資料
@[card](https://zenn.dev/gunjo/articles/8450e69537dbb6)
:::

## 結論

:::message

### 1. Zenn で自分の最新記事を取得する

結果: **できた**
補足: 本記事で解説してます

---

### 2. 調べたら分かりそうなこと

結果: **できた**
補足: 自社の社長の名前を調べて正しかった

---

### 3. 自社の勤怠管理の操作

結果: **できなかった**
補足: 自社の勤怠管理を検索したが、オープンな情報ではないので見つけられない。
また、たまたま名前が同じサービスを検索結果から見つけてログインページを探すが迷走していた。

---

### 4. 自社の勤怠管理の操作 + URL を教える

結果: **指定した勤怠管理を開くことができ、適当な ID と PW を入力してログインしようとした**
補足: ログイン試行回数は、3 回でタイムアウトになった

---

### 5. 自社の勤怠管理の操作 + URL を教える + ID と PW を指定

結果: **指定した ID と PW でログインをしようとした**
補足: ID と PW は、11111111111, XXXXXXXXX にしております。

---

### 6. HTML の要素がぐちゃぐちゃな Web アプリを操作できるのか

結果: **できたが....**
補足: 表形式のデータは、一つの文字列として返ってきた

---

### 7. 業務知識が必要な Web アプリを操作できるのか

結果: **できた**
補足: 業務知識がない人が操作するには難しい Web アプリを最短で操作することが出来た
:::

## 0. 前提条件

各システム/ライブラリー等の ver

```bash
$ python -V
Python 3.12.8

$ pip show browser-use
Name: browser-use
Version: 0.1.27
Summary: Make websites accessible for AI agents
Home-page:
Author: Gregor Zunic
Author-email:
License:
Location: C:\Users\xxxxx\AppData\Local\Programs\Python\Python312\Lib\site-packages
Requires: beautifulsoup4, httpx, langchain, langchain-anthropic, langchain-aws, langchain-fireworks, langchain-google-genai, langchain-ollama, langchain-openai, lmnr, maincontentextractor, playwright, posthog, pydantic, python-dotenv, requests, setuptools
Required-by

```

## 1. インストール

```bash
pip install browser-use
playwright install
```

## 2. コーディング

```py
# 必要なライブラリをインポート
from langchain_openai import ChatOpenAI  # OpenAIのチャットモデルを利用するためのライブラリ
from browser_use import Agent  # エージェント機能を提供するライブラリ
import asyncio  # 非同期処理を使うためのライブラリ

# 非同期関数mainを定義
async def main():
    # Agentのインスタンスを作成
    agent = Agent(
        task="Zennで小倉さんというユーザーの最新記事を教えて",  # エージェントに対するタスク（質問）
        llm=ChatOpenAI(model="gpt-4o", api_key="xxxxxxxxxxxxxx"),  # GPT-4oモデルを使う（APIキーは仮の値）
    )

    # エージェントにタスクを実行させ、その結果を取得
    result = await agent.run()

    # 結果をコンソールに出力
    print(result)

# 非同期関数mainを実行
asyncio.run(main())

```

## 3. 過程

:::details 実行結果

```bash
$ python main.py
INFO     [browser_use] BrowserUse logging setup complete with level info
INFO     [root] Anonymized telemetry enabled. See https://github.com/browser-use/browser-use for more information.
INFO     [agent] 🚀 Starting task: Zennで小倉さんというユーザーの最新記事を教えて
INFO     [agent]
📍 Step 1
INFO     [agent] 🤷 Eval: Unknown - This is a new task.
INFO     [agent] 🧠 Memory: Need to find the latest article by a user named 小倉 on Zenn.
INFO     [agent] 🎯 Next goal: Search for the latest article by 小倉 on Zenn.
INFO     [agent] 🛠️  Action 1/1: {"search_google":{"query":"site:zenn.dev 小倉"}}
INFO     [controller] 🔍  Searched for "site:zenn.dev 小倉" in Google
INFO     [agent]
📍 Step 2
INFO     [agent] 👍 Eval: Success - Successfully found the latest articles by 小倉 on Zenn.
INFO     [agent] 🧠 Memory: Found latest articles by 小倉 on Zenn from search results.
INFO     [agent] 🎯 Next goal: Extract the latest article details from Google search results.
INFO     [agent] 🛠️  Action 1/2: {"click_element":{"index":30}}
INFO     [agent] 🛠️  Action 2/2: {"extract_content":{"include_links":true}}
INFO     [controller] 🖱️  Clicked button with index 30: 小倉あん🥝さんの記事一覧
INFO     [controller] 📄  Extracted page as markdown
: [tech](/tech-or-idea)[📄[ドキュメント]
わかりやすい文章を書くには14日前3](/aew2sbee/articles/document-write)[tech](/tech-or-
idea)[🎭[Playwright] 0から勉強して導入した話18日前4](/aew2sbee/articles/playwright-
implementation)[tech](/tech-or-idea)[🍃[Tailwind CSS]
ボタンUI19日前2](/aew2sbee/articles/tailwind-css-button-ui)[tech](/tech-or-
idea)[🍃[Tailwind CSS] ログイン画面UI2ヶ月前](/aew2sbee/articles/tailwind-css-login-
ui)[tech](/tech-or-idea)[🍃[Tailwind CSS]
ロードUI/スケルトンUI2ヶ月前1](/aew2sbee/articles/tailwind-css-load-ui)[tech](/tech-or-
idea)[🎭[Playwright] CI/CDで社内環境に対してGitHub ActionsのIPを許可する in
GCP編2ヶ月前2](/aew2sbee/articles/playwright-ci-cd-gcp)[tech](/tech-or-
idea)[🍃[Tailwind CSS] 私なりのチートシート2ヶ月前2](/aew2sbee/articles/tailwind-css-cheat-
sheet)[tech](/tech-or-idea)[🍃[Tailwind CSS]
n行目以降の文章は「...」と表示する2ヶ月前](/aew2sbee/articles/tailwind-css-line-
clamp)[tech](/tech-or-idea)[🧪[テスト] pictの使い方2ヶ月前](/aew2sbee/articles/test-how-
to-use-pict)[tech](/tech-or-idea)[🧪[テスト]
Windowsにpictを使えるようにする方法2ヶ月前](/aew2sbee/articles/test-pict-
install)[tech](/tech-or-idea)[🎭[Playwright]
複数の要素からn番目を取得する3ヶ月前](/aew2sbee/articles/playwright-advanced-locator-
nth)[tech](/tech-or-idea)[🎭[Playwright]
高度なロケーターのfilter()種類と使い方3ヶ月前](/aew2sbee/articles/playwright-advanced-locator-
filter)[tech](/tech-or-idea)[🎭[Playwright]
ロケーターの種類と使い方3ヶ月前1](/aew2sbee/articles/playwright-locator)[tech](/tech-or-
idea)[🥝[体験談] 未完成な社内図書館アプリ4ヶ月前3](/aew2sbee/articles/experience-
sandbooks)[idea](/tech-or-idea)[📖[2025年1月] 私が読んだ技術書たち
(39冊)4ヶ月前](/aew2sbee/articles/book-record-of-reading)[tech](/tech-or-
idea)[🛡[TypeScript] メンテしやすいコードの書き方4ヶ月前2](/aew2sbee/articles/typescript-object-
orientation)[tech](/tech-or-idea)[🛡[TypeScript]
オブジェクト指向らしい設計にする方法4ヶ月前](/aew2sbee/articles/typescript-coding-rule-
object)[tech](/tech-or-idea)[🗃️[SQL] 私なりのチートシート4ヶ月前](/aew2sbee/articles/sql-
cheat-sheet)[tech](/tech-or-idea)[🙆‍♀️[JavaScript]
チェックボックスによるボタンの活性/非活性の切り替え4ヶ月前](/aew2sbee/articles/javascript-
checkbox)[tech](/tech-or-idea)[🐙[GitHub]
fork元のレポジトリーをupstreamに設定する4ヶ月前](/aew2sbee/articles/github-
upstream)[tech](/tech-or-idea)[🐙[GitHub]
Reviewersが自分であるPRを表示する4ヶ月前](/aew2sbee/articles/github-reviewers-
me)[tech](/tech-or-idea)[🐙[GitHub Actions] Pull
Request時に自動テスト/prettier/ESLintを実行する4ヶ月前](/aew2sbee/articles/github-ci-
cd)[tech](/tech-or-idea)[🐼[bash]
シェルスクリプトでgcloudが実行できない4ヶ月前](/aew2sbee/articles/gcloud-bash-sh-
file)[idea](/tech-or-idea)[📖[思考法] 生産性を上げる行動4ヶ月前](/aew2sbee/articles/book-
increase-productivity)[tech](/tech-or-idea)[🛡[TypeScript]
残りの引数(...hoge)を配列に格納する4ヶ月前](/aew2sbee/articles/typescript-rest-
parameter)[tech](/tech-or-idea)[🛡[TypeScript]
インターフェース(interface)を継承する4ヶ月前](/aew2sbee/articles/typescript-
interface)[tech](/tech-or-idea)[🛡[TypeScript]
処理しない関数には、戻り値の型(never)を使う4ヶ月前](/aew2sbee/articles/typescript-return-type-
never)[tech](/tech-or-idea)[🛡[Jest]
mockモジュールを使ってテストを行う4ヶ月前](/aew2sbee/articles/typescript-jest-mock)[tech](/tech-
or-idea)[🛡[TypeScript]
何も返さない関数には、戻り値の型(void)を使う4ヶ月前](/aew2sbee/articles/typescript-return-type-
void)[tech](/tech-or-idea)[🛡[TypeScript]
配列の末尾の値を取得する4ヶ月前](/aew2sbee/articles/typescript-list-end)[tech](/tech-or-
idea)[🛡[TypeScript] Jsonファイルを直接インポートする4ヶ月前](/aew2sbee/articles/typescript-
import-json)[tech](/tech-or-idea)[🛡[TypeScript]
関数の引数を必須ではなくオプションにする4ヶ月前](/aew2sbee/articles/typescript-option-
parameter)[tech](/tech-or-idea)[🛡[TypeScript]
合併型の配列(複数の型が含まれているを配列)の型定義4ヶ月前](/aew2sbee/articles/typescript-list-multi-
type)[tech](/tech-or-idea)[🛡[TypeScript]
Jsonファイルを直接インポートする4ヶ月前](/aew2sbee/articles/typescript-json-file-
import)[tech](/tech-or-idea)[🛡[Jest]
Typescript環境でカバレッジレポートを表示する4ヶ月前](/aew2sbee/articles/typescript-jest-
coverage)[tech](/tech-or-idea)[🛡[TypeScript]
インターフェース(interface)と型エイリアス(type)の違い4ヶ月前](/aew2sbee/articles/typescript-
interface-vs-type)[tech](/tech-or-idea)[🛡[TypeScript]
インターフェース(interface)で読み取り専用にする4ヶ月前](/aew2sbee/articles/typescript-interface-
readonly)[tech](/tech-or-idea)[🛡[TypeScript]
関数内で扱う型を引数のように受け取るジェネリクス関数4ヶ月前](/aew2sbee/articles/typescript-generics-
func)[tech](/tech-or-idea)[⚡[Next.js]
tsconfig.jsonの設定4ヶ月前2](/aew2sbee/articles/nextjs-tsconfig-json)[tech](/tech-
or-idea)[⚡[Next.js] Prettierの導入4ヶ月前1](/aew2sbee/articles/nextjs-
prettier)[tech](/tech-or-idea)[🎫[Linux]
基本コマンド操作(CLI)4ヶ月前](/aew2sbee/articles/linux-command-line-
interface)[tech](/tech-or-idea)[🎡[Jest]
Typescript環境でカバレッジレポートを表示する4ヶ月前](/aew2sbee/articles/jest-
coverage)[tech](/tech-or-idea)[🍧[JavaScript]
一文字ずつ時間差で表示するアニメーション4ヶ月前1](/aew2sbee/articles/javascript-timelag-
animation)[tech](/tech-or-idea)[⚡[Next.js] Root
layoutファイルでheaderタグとfooterタグを共通化する4ヶ月前1](/aew2sbee/articles/nextjs-root-
layout-file)[tech](/tech-or-idea)[🛡[TypeScript]
それぞれで型定義ではなく、呼び出しシグネチャを使う4ヶ月前](/aew2sbee/articles/typescript-call-
signature)[tech](/tech-or-idea)[⚡[Next.js]
LinkコンポーネントとuseRouterの違い4ヶ月前](/aew2sbee/articles/nextjs-link-
userouter)[tech](/tech-or-idea)[🛡[TypeScript]
非常に大きな整数はbigint型を使う4ヶ月前](/aew2sbee/articles/typescript-bigint-
type)[tech](/tech-or-idea)[🛡[TypeScript]
エラー処理/例外処理の3つの方法4ヶ月前](/aew2sbee/articles/typescript-error-
handling)[tech](/tech-or-idea)[⚡[Next.js] プロジェクトの始め方-
開発モード起動4ヶ月前](/aew2sbee/articles/nextjs-project-dev)[tech](/tech-or-
idea)[⚡[Next.js] page.tsxについて4ヶ月前](/aew2sbee/articles/nextjs-page-
tsx)[tech](/tech-or-idea)[⚡[Next.js] Server Componet とClient
Componentとは？4ヶ月前1](/aew2sbee/articles/nextjs-server-componet-client-
component)[tech](/tech-or-idea)[⚡[Next.js]
fetch関数で取得したデータをキャッシュにしない4ヶ月前](/aew2sbee/articles/nextjs-fetch-no-
store)[tech](/tech-or-idea)[🧪[テスト]
今よりも単体テストを良くする考え方4ヶ月前5](/aew2sbee/articles/test-high-performance-at-minimal-
cost)[idea](/tech-or-idea)[🔎[レビュー] 指摘漏れを減らす方法5ヶ月前](/aew2sbee/articles/how-to-
review)[tech](/tech-or-idea)[🔐[セキュリティ対策]
安全なパスワードにするには？6ヶ月前](/aew2sbee/articles/book-secure-web-app-
password)[tech](/tech-or-idea)[🧜‍♀️[React]
metaタグを理解する2023/12/141](/aew2sbee/articles/react-meta-tag)[tech](/tech-or-
idea)[🛡[TypeScript] enum（列挙型）2023/12/12](/aew2sbee/articles/typescript-
enum)[tech](/tech-or-idea)[🩹[API設計]
良いAPI(URL)とは2023/09/093](/aew2sbee/articles/oreilly-web-api-the-good-
parts)[tech](/tech-or-idea)[🎨[デザイン] Atomic
Designをデジタル庁のサイトで理解する2023/08/242](/aew2sbee/articles/atomic-design-for-
digital)[tech](/tech-or-idea)[🚀[Django]
db.sqlite3からpostgresqlに変更する方法2023/08/201](/aew2sbee/articles/django-rest-
framework-postgres)[tech](/tech-or-idea)[🦇[React]
useStateの使い方2023/08/202](/aew2sbee/articles/react-usestate)[tech](/tech-or-
idea)[💨[AWS]AWS
VaultとTerraformでEC2を作成する2023/08/171](/aew2sbee/articles/terraform-aws-vault-
ec2)[tech](/tech-or-idea)[☁️[AWS] PowerShellにAWS
Vaultコマンドを使えるようにする2023/08/16](/aew2sbee/articles/aws-vault-install-to-
powershell)[tech](/tech-or-idea)[🐳[Docker] Docker
DeskTopのインストール2023/08/09](/aew2sbee/articles/docker-desktop-
install)[tech](/tech-or-idea)[🐍[Python]
Python3.10.11をインストールする2023/08/081](/aew2sbee/articles/python-3-10-11-install)[tech](/tech-
or-idea)[🧪[テスト] サンプル仕様からテストパターンを考える2023/08/061](/aew2sbee/articles/thinking-
test-pattern)[tech](/tech-or-idea)[🚀[Django]
modelsのField一覧2023/08/01](/aew2sbee/articles/django-rest-framework-
models)[tech](/tech-or-idea)[🚀[Django] django rest
framework環境構築/インストール2023/07/291](/aew2sbee/articles/django-rest-framework-
install)[tech](/tech-or-
idea)[🤦【備忘録】未経験者チームのリーダーをやって2ヶ月経過2023/07/191](/aew2sbee/articles/first-time-
leader)[tech](/tech-or-idea)[☁️[AWS]
IAMで読み取り専用ユーザー本人がMFAの設定が出来るようにする方法2023/07/19](/aew2sbee/articles/aws-ec2-iam-
role)[tech](/tech-or-idea)[☁️[AWS]
なぜ、IAMロールでスイッチロールする必要があるのか？2023/07/19](/aew2sbee/articles/aws-ec2-iam-why-
role)[tech](/tech-or-
idea)[❄[React]ローカル環境にTypeScriptで動かす環境を構築2023/07/182](/aew2sbee/articles/react-
install)[tech](/tech-or-idea)[☁️[AWS]
IAMで読み取り専用ユーザーを作成する2023/07/131](/aew2sbee/articles/aws-ec2-iam-create-
user)[tech](/tech-or-idea)[🐳[Docker]
Reactの環境構築2023/07/072](/aew2sbee/articles/docker-react-env)[tech](/tech-or-
idea)[🐳[Docker] 少しだけ分かった気がする「イラスト付き」2023/07/051](/aew2sbee/articles/docker-
comprehension)[tech](/tech-or-idea)[🐍[Python]
matplotlibで折れ線グラフを描く2023/07/02](/aew2sbee/articles/python-matplotlib-
plot)[tech](/tech-or-idea)[🐍[Python]
matplotlibで円グラフを描く2023/07/02](/aew2sbee/articles/python-matplotlib-
pie)[tech](/tech-or-idea)[🐍[Python]
matplotlibでヒストグラムを描く2023/07/02](/aew2sbee/articles/python-matplotlib-
hist)[tech](/tech-or-idea)[🐍[Python]
matplotlibで棒グラフを描く2023/07/02](/aew2sbee/articles/python-matplotlib-
bar)[tech](/tech-or-idea)[🐍[Python]
matplotlibで直線グラフを描く2023/07/02](/aew2sbee/articles/python-
matplotlib-45graph)[idea](/tech-or-idea)[🌆[SES]
良い転職したいならテックブログを書こう!!2023/07/012](/aew2sbee/articles/tech-blog-for-job-
change)[tech](/tech-or-idea)[☁️[AWS]
VSCodeからEC2クラウドにSSH接続する2023/07/0112](/aew2sbee/articles/aws-
ec2-vscode)[tech](/tech-or-idea)[☁️[AWS]
EC2でWebサーバーを構築する2023/06/30](/aew2sbee/articles/aws-ec2-web-
server)[tech](/tech-or-idea)[🐍[Python]
CSVファイルを読み込む2パターン2023/06/301](/aew2sbee/articles/python-
csv-2pattern)[tech](/tech-or-idea)[🛡[TypeScript]
数字型配列の合計値を求めるreduce関数2023/06/28](/aew2sbee/articles/typescript-
reduce)[tech](/tech-or-idea)[🛡[TypeScript]
配列に指定された条件を判定するsome関数2023/06/273](/aew2sbee/articles/typescript-
some)[tech](/tech-or-idea)[🛡[TypeScript]
配列の各要素に対して処理できるmap関数2023/06/272](/aew2sbee/articles/typescript-
map)[tech](/tech-or-idea)[🛡[TypeScript]
配列の含有を判定するincludes関数2023/06/27](/aew2sbee/articles/typescript-
includes)[tech](/tech-or-idea)[🛡[TypeScript]
配列の条件に一致する最初の要素を取得するfind関数2023/06/27](/aew2sbee/articles/typescript-
find)[tech](/tech-or-idea)[🛡[TypeScript]
配列の条件を満たす要素で新しい配列を作成するfilter関数2023/06/27](/aew2sbee/articles/typescript-
filter)[tech](/tech-or-idea)[🛡[TypeScript]
配列の全ての要素を判定するevery関数2023/06/272](/aew2sbee/articles/typescript-
every)[tech](/tech-or-idea)[🐍[Python]
pandasで範囲内のデータ数を求める2023/06/27](/aew2sbee/articles/python-pandas-
value_counts)[tech](/tech-or-idea)[🐍[Python]
pandasで中央値を求める2023/06/27](/aew2sbee/articles/python-pandas-
median)[tech](/tech-or-idea)[🐍[Python]
pandasで平均値を求める2023/06/271](/aew2sbee/articles/python-pandas-mean)[tech](/tech-
or-idea)[🛡[TypeScript]
オブジェクト(object)を配列に変換する2023/06/27](/aew2sbee/articles/typescript-object-to-
list)[tech](/tech-or-idea)[🐍[Python]
seleniumの仕様が変わっていた!?(2023/06時点)2023/06/258](/aew2sbee/articles/python-error-
selenium)[tech](/tech-or-idea)[🚀[Django]
環境構築/インストール2023/06/23](/aew2sbee/articles/django-install)[tech](/tech-or-
idea)[🛡[TypeScript]
これ('||')とそれ('&&')って何？2023/06/232](/aew2sbee/articles/typescript-or-
and)[tech](/tech-or-idea)[🛡[TypeScript]
ver/let/constの違い2023/06/221](/aew2sbee/articles/typescript-ver-let-
const)[tech](/tech-or-idea)[🛡[TypeScript]
これ('=>')って何？2023/06/20](/aew2sbee/articles/typescript-arrow)[tech](/tech-or-
idea)[🧵[yarn] yarn: error: no such option:
XXXを解決2023/06/201](/aew2sbee/articles/yarn-error-no-such-option)[tech](/tech-
or-idea)[⛰[Prisma] npx prisma db
seedが実行できない2023/06/202](/aew2sbee/articles/prisma-seed-error)[tech](/tech-or-
idea)[🐍[Python] ModbusTCP通信環境を構築する2023/06/132](/aew2sbee/articles/python-
pymodbustcp)[tech](/tech-or-idea)[🐍[Python]
APIで郵便番号から住所を取得する方法2023/06/131](/aew2sbee/articles/python-
zipcode)[tech](/tech-or-
idea)[📉【備忘録】ステータスコードのまとめ2023/06/13](/aew2sbee/articles/general-
statuscode)[idea](/tech-or-
idea)[📖【備忘録】リーン・スタートアップ2023/06/13](/aew2sbee/articles/book-
leanstartup)[idea](/tech-or-
idea)[📖【備忘録】プリンシプルオブプログラミングの感想2023/06/11](/aew2sbee/articles/book-
principlesofprogramming)[tech](/tech-or-idea)[🐍[Python]
VScodeでmatplotlibのグラフを表示する方法2023/06/09](/aew2sbee/articles/python-matplotlib-
vscode)[tech](/tech-or-idea)[🐍[Python]
見づらいJsonデータを解決する方法2023/06/091](/aew2sbee/articles/python-pprint)[tech](/tech-
or-idea)[🐙[Git] よく使うGitコマンドのまとめ2023/06/07](/aew2sbee/articles/git-
basiccommand)



INFO     [agent]
📍 Step 3
INFO     [agent] 👍 Eval: Success - Accessed the page with the articles of 小倉さん.
INFO     [agent] 🧠 Memory: On the profile page of 小倉さん on Zenn, viewing the list of articles.
INFO     [agent] 🎯 Next goal: Identify the latest article by 小倉さん and provide its details.
INFO     [agent] 🛠️  Action 1/1: {"done":{"text":"The latest article by 小倉さん on Zenn is '[ドキュメント] わかりやすい文章を書くには', published 14 days ago."}}
INFO     [agent] 📄 Result: The latest article by 小倉さん on Zenn is '[ドキュメント] わかりやすい文章を書くには', published 14 days ago.
INFO     [agent] ✅ Task completed successfully
INFO     [agent] Created GIF at agent_history.gif
AgentHistoryList(all_results=[ActionResult(is_done=False, extracted_content='🔍  Searched for "site:zenn.dev 小倉" in Google', error=None, include_in_memory=True), ActionResult(is_done=False, extracted_content='🖱️  Clicked button with index 30: 小倉あん🥝さんの記事一覧', error=None, include_in_memory=True), ActionResult(is_done=Fal
se, extracted_content="📄  Extracted page as markdown\n: [tech](/tech-or-idea)[📄[ドキュメント]\nわかりやすい文章を書くには14日前3](/aew2sbee/articles/document-write)[tech](/tech-or-\nidea)[🎭[Playwright] 0から勉強して導入した話18日前4](/aew2sbee/articles/playwright-\nimplementation)[tech](/tech-or-idea)[🍃[Tailwind CSS]\nボタンUI19日前2](/aew2sbee/articles/tailwind-css-button-ui)[tech](/tech-or-\nidea)[🍃[Tailwind CSS] ログイン画面UI2ヶ月前](/aew2sbee/articles/tailwind-css-login-\nui)[tech](/tech-or-idea)[🍃[Tailwind CSS]\nロードUI/スケルトンUI2ヶ月前1](/aew2sbee/articles/tailwind-css-load-ui)[tech](/tech-or-\nidea)[🎭[Playwright] CI/CDで社内環境に対してGitHub ActionsのIPを許可する in\nGCP編2ヶ月前2](/aew2sbee/articles/playwright-ci-cd-gcp)[tech](/tech-or-\nidea)[🍃[Tailwind CSS] 私なりのチートシート2ヶ月前2](/aew2sbee/articles/tailwind-css-cheat-\nsheet)[tech](/tech-or-idea)[🍃[Tailwind CSS]\nn行目以降の文章は「...」と表示する2ヶ月前](/aew2sbee/articles/tailwind-css-line-\nclamp)[tech](/tech-or-idea)[🧪[テスト] pictの使い方2ヶ月前](/aew2sbee/articles/test-how-\nto-use-pict)[tech](/tech-or-idea)[🧪[テスト]\nWindowsにpictを使えるようにする方法2ヶ月前](/aew2sbee/articles/test-pict-\ninstall)[tech](/tech-or-idea)[🎭[Playwright]\n複数の要素からn番目を取得する3ヶ月前](/aew2sbee/articles/playwright-advanced-locator-\nnth)[tech](/tech-or-idea)[🎭[Playwright]\n高度なロケーターのfilter()種類と使い方3ヶ月前](/aew2sbee/articles/playwright-advanced-locator-\nfilter)[tech](/tech-or-idea)[🎭[Playwright]\nロケーターの種類と使い方3ヶ月前1](/aew2sbee/articles/playwright-locator)[tech](/tech-or-\nidea)[🥝[体験談] 未完成な社内図書館アプリ4ヶ月前3](/aew2sbee/articles/experience-\nsandbooks)[idea](/tech-or-idea)[📖[2025年1月] 私が読んだ技術書たち\n(39冊)4ヶ月前](/aew2sbee/articles/book-record-of-reading)[tech](/tech-or-\nidea)[🛡[TypeScript] メンテしやすいコードの書き方4ヶ月前2](/aew2sbee/articles/typescript-object-\norientation)[tech](/tech-or-idea)[🛡[TypeScript]\nオブジェクト指向らしい   設計にする方法4ヶ月前](/aew2sbee/articles/typescript-coding-rule-\nobject)[tech](/tech-or-idea)[🗃️[SQL] 私なりのチートシート4ヶ月前](/aew2sbee/articles/sql-\ncheat-sh
eet)[tech](/tech-or-idea)[🙆\u200d♀️[JavaScript]\nチェックボックスによるボタンの活性/非活性の切り替え4ヶ月前](/aew2sbee/articles/javascript-\ncheckbox)[tech](/tech-or--idea)[🐙[GitHub]\nfork元のレポジトリーをupstreamに設定する4ヶ月前](/aew2sbee/articles/github-\nupstream)[tech](/tech-or-idea)[🐙[GitHub]\nReviewersが自分であるPRを表示する4ヶ月前](/aew2sbee/articles/github-reviewers-\nme)[tech](/tech-or-idea)[🐙[GitHub Actions] Pull\nRequest時に自動テスト/prettier/ESLintを実行する4ヶ月前](/aew2sbee/articles/github-ci-\ncd)[tech](/tech-or-idea)[🐼[bash]\nシェルスクリプトでgcloudが実行できない4ヶ月前](/aew2sbee/articles/gcloud-bash-sh-\nfile)[idea](/tech-or-idea)[�[[思考法] 生産性を上げる行動4ヶ月前](/aew2sbee/articles/book-\nincrease-productivity)[tech](/tech-or-idea)[🛡[TypeScript]\n残りの引数(...hoge)を配列に格納する4ヶ月前](
/aew2sbee/articles/typescript-rest-\nparameter)[tech](/tech-or-idea)[🛡[TypeScript]\nインターフェース(interface)を継承する4ヶ月前](/aew2sbee/articles/typescript-\ninte
rface)[tech](/tech-or-idea)[🛡[TypeScript]\n処理しない関数には、戻り値の型(never)を使う4ヶ月前](/aew2sbee/articles/typescript-return-type-\nnever)[tech](/tech-or-idea)
[🛡[Jest]\nmockモジュールを使ってテストを行う4ヶ月前](/aew2sbee/articles/typescript-jest-mock)[tech](/tech-\nor-idea)[🛡[TypeScript]\n何も返さない関数には、戻り値の型(
void)を使う4ヶ月前](/aew2sbee/articles/typescript-return-type-\nvoid)[tech](/tech-or-idea)[🛡[TypeScript]\n配列の末尾の値を取得する4ヶ月前](/aew2sbee/articles/typescri
pt-list-end)[tech](/tech-or-\nidea)[🛡[TypeScript] Jsonファイルを直接インポートする4ヶ月前](/aew2sbee/articles/typescript-\nimport-json)[tech](/tech-or-idea)[🛡[TypeSc
ript]\n関数の引数を必須ではなくオプションにする4ヶ月前](/aew2sbee/articles/typescript-option-\nparameter)[tech](/tech-or-idea)[🛡[TypeScript]\n合併型の配列(複数の型が  含まれているを配列)の型定義4ヶ月前](/aew2sbee/articles/typescript-list-multi-\ntype)[tech](/tech-or-idea)[🛡[TypeScript]\nJsonファイルを直接インポートする4ヶ月前](/aew
2sbee/articles/typescript-json-file-\nimport)[tech](/tech-or-idea)[🛡[Jest]\nTypescript環境でカバレッジレポートを表示する4ヶ月前](/aew2sbee/articles/typescript-jest-\n
coverage)[tech](/tech-or-idea)[🛡[TypeScript]\nインターフェース(interface)と型エイリアス(type)の違い4ヶ月前](/aew2sbee/articles/typescript-\ninterface-vs-type)[tech](/
tech-or-idea)[🛡[TypeScript]\nインターフェース(interface)で読み取り専用にする4ヶ月前](/aew2sbee/articles/typescript-interface-\nreadonly)[tech](/tech-or-idea)[🛡[TypeS
cript]\n関数内で扱う型を引数のように受け取るジェネリクス関数4ヶ月前](/aew2sbee/articles/typescript-generics-\nfunc)[tech](/tech-or-idea)[⚡[Next.js]\ntsconfig.jsonの設定4ヶ月前2](/aew2sbee/articles/nextjs-tsconfig-json)[tech](/tech-\nor-idea)[⚡[Next.js] Prettierの導入4ヶ月前1](/aew2sbee/articles/nextjs-\nprettier)[tech](/tech-or-idea)[🎫[Linux]\n基本コマンド操作(CLI)4ヶ月前](/aew2sbee/articles/linux-command-line-\ninterface)[tech](/tech-or-idea)[🎡[Jest]\nTypescript環境でカバレッジレポートを表示する4ヶ月前](/aew2sbee/articles/jest-\ncoverage)[tech](/tech-or-idea)[🍧[JavaScript]\n一文字ずつ時間差で表示するアニメーション4ヶ月前1](/aew2sbee/articles/javascript-timelag-\nanimation)[tech](/tech-or-idea)[⚡[Next.js] Root\nlayoutファイルでheaderタグとfooterタグを共通化する4ヶ月前1](/aew2sbee/articles/nextjs-root-\nlayout-file)[tech](/tech-or-idea)[🛡[TypeScript]\nそれぞれで型定義ではなく、呼び出しシグネチャを使う4ヶ月前](/aew2sbee/articles/typescript-call-\nsignature)[tech](/tech-or-idea)[⚡[N
ext.js]\nLinkコンポーネントとuseRouterの違い4ヶ月前](/aew2sbee/articles/nextjs-link-\nuserouter)[tech](/tech-or-idea)[🛡[TypeScript]\n非常に大きな整数はbigint型を使う4
ヶ月前](/aew2sbee/articles/typescript-bigint-\ntype)[tech](/tech-or-idea)[🛡[TypeScript]\nエラー処理/例外処理の3つの方法4ヶ月前](/aew2sbee/articles/typescript-error-\n
handling)[tech](/tech-or-idea)[⚡[Next.js] プロジェクトの始め方-\n開発モード起動4ヶ月前](/aew2sbee/articles/nextjs-project-dev)[tech](/tech-or-\nidea)[⚡[Next.js] page.tsxについて4ヶ月前](/aew2sbee/articles/nextjs-page-\ntsx)[tech](/tech-or-idea)[⚡[Next.js] Server Componet とClient\nComponentとは？4ヶ月前1](/aew2sbee/articles/nextjs-server-componet-client-\ncomponent)[tech](/tech-or-idea)[⚡[Next.js]\nfetch関数で取得したデータをキャッシュにしない4ヶ月前](/aew2sbee/articles/nextjs-fetch-no-\nstore)[tech](/tech-or-idea)[🧪[テスト]\n今よりも単体テストを良くする考え方4ヶ月前5](/aew2sbee/articles/test-high-performance-at-minimal-\ncost)[idea](/tech-or-idea)[🔎[レ ビュー] 指摘漏れを減らす方法5ヶ月前](/aew2sbee/articles/how-to-\nreview)[tech](/tech-or-idea)[🔐[セキュリティ対策]\n安全なパスワードにするには？6ヶ月前](/aew2sbee/articles/book-secure-web-app-\npassword)[tech](/tech-or-idea)[🧜\u200d♀️[React]\nmetaタグを理解する2023/12/141](/aew2sbee/articles/react-meta-tag)[tech](/tech-or-\nidea)[��[[TypeScript] enum（列挙型）2023/12/12](/aew2sbee/articles/typescript-\nenum)[tech](/tech-or-idea)[🩹[API設計]\n良いAPI(URL)とは2023/09/093](/aew2sbee/articles/oreilly-web-api-the-good-\nparts)[tech](/tech-or-idea)[🎨[デザイン] Atomic\nDesignをデジタル庁のサイトで理解する2023/08/242](/aew2sbee/articles/atomic-design-for-\ndigital)[tech](/tech-or-idea)[🚀[Django]\ndb.sqlite3からpostgresqlに変更する方法2023/08/201](/aew2sbee/articles/django-rest-\nframework-postgres)[tech](/tech-or-idea)[🦇[React]\nuseStateの使い方2023/08/202](/aew2sbee/articles/react-usestate)[tech](/tech-or-\nidea)[💨[AWS]AWS\nVaultとTerraformでEC2を作成する2023/08/171](/aew2sbee/articles/terraform-aws-vault-\nec2)[tech](/tech-or-idea)[☁️[AWS] PowerShellにAWS\nVaultコマンドを使えるようにする2023/08/16](/aew2sbee/articles/aws-vault-install-to-\npowershell)[ttech](/tech-or-idea)[🐳[Docker] Docker\nDeskTopのインストール2023/08/09](/aew2sbee/articles/docker-desktop-\ninstall)[tech](/tech-or-idea)[🐍[Python]\nPython3.10.11をインストールする2023/08/081](/aew2sbee/articles/python-3-10-11-install)[tech](/tech-\nor-idea)[🧪[テスト] サンプル仕様からテストパターンを考える2023/08/061](/aew2sbee/articles/thinking-\ntest-pattern)[tech](/tech-or-idea)[🚀[Django]\nmodelsのField一覧2023/08/01](/aew2sbee/articles/django-rest-framework-\nmodels)[tech](/tech-or-idea)[�[[Django] django rest\nframework環境構築/インストール2023/07/291](/aew2sbee/articles/django-rest-framework-\ninstall)[tech](/tech-or-\nidea)[🤦【備忘録】未経験者チームのリーダーをやって2ヶ月経過2023/07/191](/aew2sbee/articles/first-time-\nleader)[tech](/tech-or-idea)[☁️[AWS]\nIAMで読み取り専用ユーザー本人がMFAの設定が出来るようにす  る方法2023/07/19](/aew2sbee/articles/aws-ec2-iam-\nrole)[tech](/tech-or-idea)[☁️[AWS]\nなぜ、IAMロールでスイッチロールする必要があるのか？2023/07/19](/aew2sbee/articlees/aws-ec2-iam-why-\nrole)[tech](/tech-or-\nidea)[❄[React]ローカル環境にTypeScriptで動かす環境を構築2023/07/182](/aew2sbee/articles/react-\ninstall)[tech](/tech-or-idea)[☁️[AWS]\nIAMで読み取り専用ユーザーを作成する2023/07/131](/aew2sbee/articles/aws-ec2-iam-create-\nuser)[tech](/tech-or-idea)[🐳[Docker]\nReactの環境構築2023/07/072](//aew2sbee/articles/docker-react-env)[tech](/tech-or-\nidea)[🐳[Docker] 少しだけ分かった気がする「イラスト付き」2023/07/051](/aew2sbee/articles/docker-\ncomprehension)[tech](/tech-or-idea)[🐍[Python]\nmatplotlibで折れ線グラフを描く2023/07/02](/aew2sbee/articles/python-matplotlib-\nplot)[tech](/tech-or-idea)[🐍[Python]\nmatplotlibで円 グラフを描く2023/07/02](/aew2sbee/articles/python-matplotlib-\npie)[tech](/tech-or-idea)[🐍[Python]\nmatplotlibでヒストグラムを描く2023/07/02](/aew2sbee/articles/python-matplotlib-\nhist)[tech](/tech-or-idea)[🐍[Python]\nmatplotlibで棒グラフを描く2023/07/02](/aew2sbee/articles/python-matplotlib-\nbar)[tech](/tech-or-idea)[🐍[Python]\nmatplotlibで直線グラフを描く2023/07/02](/aew2sbee/articles/python-\nmatplotlib-45graph)[idea](/tech-or-idea)[🌆[SES]\n良い転職したいならテックブログを書こう!!2023/07/012](/aew2sbee/articles/tech-blog-for-job-\nchange)[tech](/tech-or-idea)[☁️[AWS]\nVSCodeからEC2クラウドにSSH接続する2023/07/0112](/aew2sbee/articles/aws-\nec2-vscode))[tech](/tech-or-idea)[☁️[AWS]\nEC2でWebサーバーを構築する2023/06/30](/aew2sbee/articles/aws-ec2-web-\nserver)[tech](/tech-or-idea)[🐍[Python]\nCSVファイルを読み込む2 パターン2023/06/301](/aew2sbee/articles/python-\ncsv-2pattern)[tech](/tech-or-idea)[🛡[TypeScript]\n数字型配列の合計値を求めるreduce関数2023/06/28](/aew2sbee/articles/ty
pescript-\nreduce)[tech](/tech-or-idea)[🛡[TypeScript]\n配列に指定された条件を判定するsome関数2023/06/273](/aew2sbee/articles/typescript-\nsome)[tech](/tech-or-idea)[�
[[TypeScript]\n配列の各要素に対して処理できるmap関数2023/06/272](/aew2sbee/articles/typescript-\nmap)[tech](/tech-or-idea)[🛡[TypeScript]\n配列の含有を判定するincludes
関数2023/06/27](/aew2sbee/articles/typescript-\nincludes)[tech](/tech-or-idea)[🛡[TypeScript]\n配列の条件に一致する最初の要素を取得するfind関数2023/06/27](/aew2sbee/ar
ticles/typescript-\nfind)[tech](/tech-or-idea)[🛡[TypeScript]\n配列の条件を満たす要素で新しい配列を作成するfilter関数2023/06/27](/aew2sbee/articles/typescript-\nfilter
)[tech](/tech-or-idea)[🛡[TypeScript]\n配列の全ての要素を判定するevery関数2023/06/272](/aew2sbee/articles/typescript-\nevery)[tech](/tech-or-idea)[🐍[Python]\npandasで
範囲内のデータ数を求める2023/06/27](/aew2sbee/articles/python-pandas-\nvalue_counts)[tech](/tech-or-idea)[🐍[Python]\npandasで中央値を求める2023/06/27](/aew2sbee/articles/python-pandas-\nmedian)[tech](/tech-or-idea)[🐍[Python]\npandasで平均値を求める2023/06/271](/aew2sbee/articles/python-pandas-mean)[tech](/tech-\nor-idea)[🛡[TypeSc
ript]\nオブジェクト(object)を配列に変換する2023/06/27](/aew2sbee/articles/typescript-object-to-\nlist)[tech](/tech-or-idea)[🐍[Python]\nseleniumの仕様が変わっていた!?(2023/06時点)2023/06/258](/aew2sbee/articles/python-error-\nselenium)[tech](/tech-or-idea)[🚀[Django]\n環境構築/インストール2023/06/23](/aew2sbee/articles/django-install)[tech](/tech-or-\nidea)[🛡[TypeScript]\nこれ('||')とそれ('&&')って何？2023/06/232](/aew2sbee/articles/typescript-or-\nand)[tech](/tech-or-idea)[🛡[TypeScript]\nver/l
et/constの違い2023/06/221](/aew2sbee/articles/typescript-ver-let-\nconst)[tech](/tech-or-idea)[🛡[TypeScript]\nこれ('=>')って何？2023/06/20](/aew2sbee/articles/typescr
ipt-arrow)[tech](/tech-or-\nidea)[🧵[yarn] yarn: error: no such option:\nXXXを解決2023/06/201](/aew2sbee/articles/yarn-error-no-such-option)[tech](/tech-\nor-idea)[⛰[Prisma] npx prisma db\nseedが実行できない2023/06/202](/aew2sbee/articles/prisma-seed-error)[tech](/tech-or-\nidea)[🐍[Python] ModbusTCP通信環境を構築する2023/06/132](/aew2sbee/articles/python-\npymodbustcp)[tech](/tech-or-idea)[🐍[Python]\nAPIで郵便番号から住所を取得する方法2023/06/131](/aew2sbee/articles/python-\nzipcode)[tech](/tech-or-\nidea)[📉【備忘録】ステータスコードのまとめ2023/06/13](/aew2sbee/articles/general-\nstatuscode)[idea](/tech-or-\nidea)[📖【備忘録】リーン・スタートアップ2023/06/13](/aew2sbee/articles/book-\nleanstartup)[idea](/tech-or-\nidea)[📖【備忘録】プリンシプルオブプログラミングの感想2023/06/11](/aew2sbee/articles/book-\nprinciplesofprogramming)[tech](/tech-or-idea)[🐍[Python]\nVScodeでmatplotlibのグラフを表示する方法2023/06/09](/aew2sbee/articles/python-matplotlib-\nvscode)[tech](/tech-or-idea)[🐍[Python]\n見づらいJsonデータを解決する方法2023/06/091](/aew2sbee/articles/python-pprint)[tech](/tech-\nor-idea)[🐙[Git] よく使うGitコマンドのまとめ2023/06/07](/aew2sbee/articles/git-\nbasiccommand)\n\n\n", error=None, include_in_memory=False), ActionResult(is_done=True, extracted_content="The latest article by 小倉さん on Zenn is '[ドキュメント] わかりやすい文章を書くには', published 14 days ago.", error=None, include_in_memory=False)], all_model_outputs=[{'search_google': {'query': 'site:zenn.dev 小倉'}}, {'click_element': {'index': 30}}, {'extract_content': {'include_links': True}}, {'done': {'text': "The latest article by 小倉さん on Zenn is '[ドキュメント]  わかりやすい文章を書くには', published 14 days ago."}}])
```

:::

1. Google で「site:zenn.dev 小倉」と検索
2. 該当する検索結果をクリックする
   ![step01](/images/articles/browser-use-tutorial/step01.png)
3. 全記事のデータを取得し、最新のデータを取得した
   ![step02](/images/articles/browser-use-tutorial/step02.png)

## 4. 結果

ちゃんと最新の記事のタイトルを取得することは出来ました！

```json
{
  "done": {
    "text": "The latest article by 小倉さん on Zenn is '[ドキュメント] わかりやすい文章を書くには', published 14 days ago."
  }
}
```
