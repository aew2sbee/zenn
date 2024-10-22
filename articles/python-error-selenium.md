---
title: '[Python] seleniumの仕様が変わっていた!?(2023/06時点)' # 記事のタイトル
emoji: '🐍' # アイキャッチとして使われる絵文字（1文字だけ）
type: 'tech' # tech: 技術記事 / idea: アイデア記事
topics: ['python', 'selenium'] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開設定（falseにすると下書き）
---

## はじめに

先日、社内のテストでログイン動作がめんどくさいと感じ
1~2 年前の記事を参考にしながら selenium で自動ログインスクリプトを作成しました。

しかし、いつからの selenium のアップデートから記述の仕様が多く変わって苦労したので、
躓いた変更点を紹介します。

### 対象読者

- selenium 初学者

### この記事でわかること

- 仕様変更後の selenium が理解できる

### 前提条件

- Python 3.10.11
- selenium 4.10.0
- webdriver-manager 3.8.6

## サンプルコード

### 1. `webdriver.Chrome('./chromedriver.exe')`が使えない

:::message alert
`webdriver.Chrome('./chromedriver.exe')`でブラウザーをインスタンス化しようとすると
下記のエラーが発生しました。

```bash
AttributeError: 'str' object has no attribute '_ignore_local_proxy'
```

:::
再現方法を紹介します。

1. ファイル構成
   `chromedriver.exe`をダウンロードして下記の通りに配置します。

```bash
.
├── chromedriver.exe
└── error_selenium001.py
```

2. `webdriver.Chrome('./chromedriver.exe')`を含めたスクリプトを作成する
   Google の検索ページを表示するスクリプトを作成

```python: error_selenium001.py
from selenium import webdriver

# ブラウザインスタンス
driver = webdriver.Chrome('./chromedriver.exe')
#特定のURLへ移動
driver.get('https://www.google.com/')

#最大待機時間を10秒にセット
driver.implicitly_wait(10)
```

3. スクリプトを実行する

```bash
$ python error_selenium001.py
Traceback (most recent call last):
  File "C:\Users\xxxxxxxx\Work\Zenn\error_selenium001.py", line 7, in <module>
    driver = webdriver.Chrome('./chromedriver.exe')
  File "C:\Users\xxxxxxxx\AppData\Local\Programs\Python\Python310\lib\site-packages\selenium\webdriver\chrome\webdriver.py", line 49, in __init__
    super().__init__(
  File "C:\Users\xxxxxxxx\AppData\Local\Programs\Python\Python310\lib\site-packages\selenium\webdriver\chromium\webdriver.py", line 60, in __init__
    ignore_proxy=self.options._ignore_local_proxy,
AttributeError: 'str' object has no attribute '_ignore_local_proxy'
```

#### じゃあどうしたらいいの？

`webdriver.Chrome('./chromedriver.exe')`を`webdriver.Chrome()`に変更します

```diff python: error_selenium001.py
from selenium import webdriver

# ブラウザインスタンス
- driver = webdriver.Chrome('./chromedriver.exe')
+ driver = webdriver.Chrome()
#特定のURLへ移動
driver.get('https://www.google.com/')

#最大待機時間を10秒にセット
driver.implicitly_wait(10)
```

### 2. `webdriver.Chrome(ChromeDriverManager().install())`が使えない

:::message alert
`webdriver.Chrome(ChromeDriverManager().install())`でブラウザーをインスタンス化しようとすると下記のエラーが発生しました。

```bash
AttributeError: 'str' object has no attribute '_ignore_local_proxy'
```

:::
再現方法を紹介します。

1. `webdriver.Chrome(ChromeDriverManager().install())`を含めたスクリプトを作成します
   Google の検索ページを表示するスクリプトを作成

```python: error_selenium002.py
from selenium import webdriver
from webdriver_manager.chrome import ChromeDriverManager

# ブラウザインスタンス
driver = webdriver.Chrome(ChromeDriverManager().install())
#特定のURLへ移動
driver.get('https://www.google.com/')

#最大待機時間を10秒にセット
driver.implicitly_wait(10)

```

2. スクリプトを実行する

```bash
$ python error_selenium002.py
[WDM] - Downloading: 100%|##########| 6.30M/6.30M [00:01<00:00, 4.48MB/s]
Traceback (most recent call last):
  File "C:\Users\xxxxxxxx\Work\Zenn\error_selenium002.py", line 7, in <module>
    driver = webdriver.Chrome(ChromeDriverManager().install())
  File "C:\Users\xxxxxxxx\AppData\Local\Programs\Python\Python310\lib\site-packages\selenium\webdriver\chrome\webdriver.py", line 49, in __init__
    super().__init__(
  File "C:\Users\xxxxxxxx\AppData\Local\Programs\Python\Python310\lib\site-packages\selenium\webdriver\chromium\webdriver.py", line 60, in __init__
    ignore_proxy=self.options._ignore_local_proxy,
AttributeError: 'str' object has no attribute '_ignore_local_proxy'
```

#### じゃあどうしたらいいの？

1. `from webdriver_manager.chrome import ChromeDriverManager`は不要なので削除します
2. `webdriver.Chrome(ChromeDriverManager().install())`を`webdriver.Chrome()`に変更します

```diff python: error_selenium002.py
from selenium import webdriver
- from webdriver_manager.chrome import ChromeDriverManager

# ブラウザインスタンス
- driver = webdriver.Chrome(ChromeDriverManager().install())
+ driver = webdriver.Chrome()
#特定のURLへ移動
driver.get('https://www.google.com/')

#最大待機時間を10秒にセット
driver.implicitly_wait(10)

```

### 3. `find_element_by_XXX`系が使えない

:::message alert
`AttributeError: 'WebDriver' object has no attribute 'find_element_by_id'`で対象サイトの`id`を取得しようとした時に下記のエラーが発生しました。

```bash
AttributeError: 'WebDriver' object has no attribute 'find_element_by_id'
```

:::
再現方法を紹介します。

1. `driver.find_element_by_id("username")`を含めたスクリプトを作成します

```python: error_selenium003.py
from selenium import webdriver

# ブラウザインスタンス
driver = webdriver.Chrome()
#特定のURLへ移動
driver.get('https://www.google.com/')

#最大待機時間を10秒にセット
driver.implicitly_wait(10)

username = driver.find_element_by_id("username")
```

2. スクリプトを実行する

```bash
$ python error_selenium003.py
Traceback (most recent call last):
  File "C:\Users\xxxxxxxx\Work\Zenn\error_selenium003.py", line 11, in <module>
    username = driver.find_element_by_id("username")
AttributeError: 'WebDriver' object has no attribute 'find_element_by_id'
```

#### じゃあどうしたらいいの？

1. `find_element`の引数に`By`が必要なため`from selenium.webdriver.common.by import By`を import します
2. `driver.find_element_by_id("username")`を`driver.find_element(By.ID, "username")`に変更します

```diff python: error_selenium003.py
from selenium import webdriver
+ from selenium.webdriver.common.by import By

# ブラウザインスタンス
driver = webdriver.Chrome()
#特定のURLへ移動
driver.get('https://www.google.com/')

#最大待機時間を10秒にセット
driver.implicitly_wait(10)

- username = driver.find_element_by_id("username")
+ username = driver.find_element(By.ID, "username")
```

#### 対比表

| 検索手段                   | 旧記述                                                    | 新記述                                                      |
| -------------------------- | --------------------------------------------------------- | ----------------------------------------------------------- |
| id                         | `.find_element_by_id("id")`                               | `.find_element(By.ID, "id")`                                |
| name                       | `.find_element_by_name("name")`                           | `.find_element(By.NAME, "name")`                            |
| xpath                      | `.find_element_by_xpath("xpath")`                         | `.find_element(By.XPATH, "xpath")`                          |
| リンクのテキスト           | `.find_element_by_link_tsxt("link text")`                 | `.find_element(By.LINK_TEXT, "link text")`                  |
| リンクのテキスト(部分一致) | `.find_element_by_partial_link_text("partial link text")` | `.find_element(By.PARTIAL_LINK_TEXT, "partial link text"))` |
| タグ名                     | `.find_element_by_tag_name("tag name")`                   | `.find_element(By.TAG_NAME, "tag name")`                    |
| クラス名                   | `.find_element_by_class_name("class name")`               | `.find_element(By.CLASS_NAME, "class name")`                |
| css セレクタ               | `.find_element_by_css_selector("css selector")`           | `.find_element(By.CSS_SELECTOR, "css selector")`            |
