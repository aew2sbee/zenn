---
title: "【自動化テスト】sharepointでテスト設計書を管理するの辞めませんか？" # 記事のタイトル
emoji: "🥝" # アイキャッチとして使われる絵文字（1文字だけ）
type: "idea" # tech: 技術記事 / idea: アイデア記事
topics: ["python", "pytest", "csv", "tsv", "test"] # タグ。["markdown", "rust", "aws"]のように指定する
published: false # 公開設定（falseにすると下書き）
---
## はじめに
約2年間Pythonで自動化テストの担当をしておりました。

表題の通り 大半のプロジェクトは、
**テスト設計書をExcelで作成し、sharepointに格納してみんなが確認できる方法**
で行っていると思います。
自分が参画していた上記のプロジェクトも同様な方法を行っておりました。

しかし、この方法では、下記のような**問題**が発生しておりました。
1. 仕様変更でテストコードを更新したけど、**テスト設計書は旧仕様のまま**
2. Excelに記載されている100個以上あるテスト項目が**網羅されているか**確認しにくい
3. テストが増えてくると、このテストコードのテスト設計書がどれか**分からない**
4. ファイルを直接開かないルールでも、誤ってファイルを開き**更新**されてしまう
5. テスト設計書にテスト結果の**記入漏れ**

上記のような問題を解決するために、方法を紹介したいと思います。

### 結論
先に結論を書かせていただきます
:::message
テスト設計書を**TSVファイル**で作成し、**テストコードと一緒**にGitで管理する
:::
になります。

### 対象読者
- テスト設計書をExcelで管理する方法から開放したい方
- Pythonの基本文法を理解している方
- pytestライブラリーの使い方を知っている方

### この記事でわかること
- テスト設計書を**TSVファイル**で作成し、**テストコードと一緒**にGitで管理する方法

### 前提条件
- Python 3.9.10


## 手順解説
### 1. ファイル構成を整える
現場のテスト数が多いので分かりやすいようにシンプルなファイル構成で解説します
```bash
$ tree -a
pytest
├─ src
│   └─ calc.py  # 今回のテスト対象
└─ test
    ├─ test_design
    │   └─ test0001.tsv  # テスト試験書(tsv)
    ├─ func
    │   └─ test_common.py  # pytestに使う共通関数
    └─ test_four_arithmetic_operations.py  # テストコード
```

### 2. テスト対象となるコードを用意する
分かりやすい例として四則演算の関数を用意しました。
```python:calc.py
class Calc():

    def four_arithmetic_operations(x, y, action=None):
        # 加算
        if action == "add":
            return x + y
        # 減算
        elif action == "sub":
            return x - y
        # 掛け算
        elif action == "mul":
            return x * y
        # 割り算
        elif action == "div":
            return x / y
        # 例外
        else:
            raise ValueError("四則演算以外の操作です")
```

### 3. テスト設計書(tsvファイル)を用意する
ヘッダー要素の説明は下記の通りです。
- No: 試験番号
- action:　四則演算の操作
- x: テスト条件1
- y: テスト条件2
- expected_value:　期待値
- note:　テスト観点や備考欄


|  No  |  action  |  x  |  y  |  expected_value  |  note  |
| ---: | :--- | :--- | :--- | :--- | :--- |
|  1  |  add  |  6  |  3  |  9  |  加算  |
|  2  |  sub  |  6  |  3  |  3  |  減算  |
|  3  |  mul  |  6  |  3  |  18  |  掛け算  |
|  4  |  div  |  6  |  3  |  2  |  割り算  |
|  5  |  test  |  6  |  3  |  四則演算以外の操作です  |  例外処理  |

```tsv:test0001.tsv
No  action    x   y  expected_value note
1   add    6   3  9 加算
2   sub    6   3  3 減算
3   mul    6   3  18    掛け算
4   div    6   3  2 割り算
5   test   6  3 四則演算以外の操作です  例外処理
```
:::message alert
ヘッダー項目は実施するテストによって適した項目に変更してください。
今回は、四則演算のテストを想定したため上記のような内容となっております。
:::

### 4. テストに利用する共通関数を用意する
各関数の説明は省略させて頂きますが、関数の一覧は紹介します。

- TSVファイルに関するクラス
    - 文字列がint型であるかを確認
    - 文字列がfloat型であるかを確認
    - csvファイルを読み込んでdict型を作成
    - 文字列のデータを適切なデータ型に変換
- テストに関するクラス
    - テスト結果確認

```python: test_common.py
import csv  # CSVを扱ライブラリーをインポートする
import sys
import pytest
sys.path.append('../')     # srcを読み込めるようにルートに追加する
from src.calc import Calc  # テスト対象のCalcクラスをimportする
from copy import copy

class Common():
    """ TSV/CSVファイルを読み込み """

    def is_int(data):
        """ 文字列がint型であるかを確認する """
        try:
            int(data, 10)
        except ValueError:
            return False
        else:
            return True

    def is_float(data):
        """ 文字列がfloat型であるかを確認する """
        try:
            float(data)
        except ValueError:
            return False
        else:
            return True

    def load_csv(csv_path):
        """ csvファイルを読み込んでdict型を作成する """
        # 各試験情報をに格納するようのlist型
        operation_list = []
        # csvファイルのパスからcsvデータを取得する
        with open(csv_path) as f:
            reader = csv.reader(f)
            for i, row in enumerate(reader):
                # 0番目はheader情報であり、それ以外はデータになる
                if i == 0:
                    # headerを保存する
                    header_row = copy(row)
                    # 取得次第次のループに進む
                    continue
                else:
                    # copyで参照渡しで同じidにならないようにしている
                    deta_row = copy(row)

                # 各試験情報のdict型を初期化
                operation = {}
                # header, detaを同時にfor文で渡してdict型を作成する
                for header, deta in zip(header_row, deta_row):
                    # csvデータは全て文字列のデータなので"convert_type"で適切なデータ型に変換する
                    operation[header] = Common.convert_type(deta)
                # 各試験情報をに格納する
                operation_list.append(operation)
            return operation_list

    def convert_type(data):
        """
        文字列のデータを適切なデータ型に変換する
            capitalizeで先頭大文字にし、データの比較を容易にする
            if分岐の比較対象をlistしている理由:比較対象を追加削除を容易にする為
        """
        # True
        if data.capitalize() in ["True"]:
            return True
        # False
        elif data.capitalize() in ["False"]:
            return False
        # None
        elif data.capitalize() in ["None", "Null"]:
            return None
        # int
        elif Common.is_int(data):
            return int(data)
        # float
        elif Common.is_float(data):
            return float(data)
        # str
        else:
            return data

class Test():
    """ テスト関連 """

    def verify_check(operation):
        """テスト結果確認"""
        try:
            # 試験情報から必要なデータを取得する
            __x = operation["x"]
            __y = operation["y"]
            __action = operation["action"]
            # 引数をテスト対象のコードに渡して結果を取得する
            result = Calc.four_arithmetic_operations(__x, __y, action=__action)
            # 期待値と一致するか確認する
            if result == operation["expected_value"]:
                print(f'result:{result} == {operation["expected_value"]}')
                return True
            else:
                print(f'result:{result} != {operation["expected_value"]}')
                return False
        except:
            # 例外処理の場合は、意図的な例外処理が発生しているかを確認する
            with pytest.raises(ValueError) as e:
                Calc.four_arithmetic_operations(__x, __y, action=__action)
            # 期待値と一致するか確認する
            if str(e.value) == operation["expected_value"]:
                print(f'result:{e.value} == {operation["expected_value"]}')
                return True
            else:
                print(f'result:{e.value} != {operation["expected_value"]}')
                return False
```

### 5. テストコードを用意する
1. `setup_pytest`関数でテスト試験書(tsv)を読み込む
2. TSVを読み込んだ情報を各テスト関数が読み込む設計にします

```python:test_four_arithmetic_operations.py
import sys
import pytest
sys.path.append('../')     # srcを読み込めるようにルートに追加する
from src.calc import Calc  # テスト対象のCalcクラスをimportする
from func.test_common import Common
from func.test_common import Test


@pytest.fixture(scope='class')
def setup_pytest():
    operation = Common.load_csv("csv/test_design.csv")

    yield operation

def test_000(setup_pytest):
    assert Test.verify_check(setup_pytest[int(sys._getframe().f_code.co_name[-1])])

def test_001(setup_pytest):
    assert Test.verify_check(setup_pytest[int(sys._getframe().f_code.co_name[-1])])

def test_002(setup_pytest):
    assert Test.verify_check(setup_pytest[int(sys._getframe().f_code.co_name[-1])])

def test_003(setup_pytest):
    assert Test.verify_check(setup_pytest[int(sys._getframe().f_code.co_name[-1])])

def test_004(setup_pytest):
    assert Test.verify_check(setup_pytest[int(sys._getframe().f_code.co_name[-1])])
```


:::message
**コラム：テスト結果確認関数を実装するメリット**
テスト結果で`NG`になり理由として多いのが、**テストコード側のミス**でした。
**テストコード側のミス**を減らすために実装しました。
1. **コーディング技術が高くない方**でも同じ品質のテストコードが作成できる
    - 説明：`test_four_arithmetic_operations.py`のように1つのテストケースのコードが出来たら複製できるように設計しました。
2. テストケース名から対象の試験を呼び出す設計
    - 説明：`sys._getframe().f_code.co_name[-1]`がテストケース名を取得できるので、この値とTSVの試験番号を紐づけました。
3. 人によるコーディングの差を埋めることが出来る
    - 説明：メンバーが共通関数を使いまわせるため、差がなくなる

:::

### 5. テストを実行する
1. カレントディレクトリを`/pytest/test`になるように移動する
2. `pytest -v test_four_arithmetic_operations.py`を実行する
3. 結果を確認する


```bash
$ pytest -v test_four_arithmetic_operations.py
================ test session starts ===================================================
platform linux -- Python 3.9.10, pytest-7.2.0, pluggy-1.0.0
rootdir: /mnt/c/Users/User/work/study/src/Python/Blog/Pytest/test
collected 5 items

test_four_arithmetic_operations.py::test_000 PASSED    [ 20%]
test_four_arithmetic_operations.py::test_001 PASSED    [ 40%] 
test_four_arithmetic_operations.py::test_002 PASSED    [ 60%]
test_four_arithmetic_operations.py::test_003 PASSED    [ 80%] 
test_four_arithmetic_operations.py::test_004 PASSED    [100%]

================ 5 passed in 0.16s ====================================================
```

5つ用意したテストパターン全て**PASS**しました。
作成したfour_arithmetic_operations関数は、期待通りの動きをする関数である事が証明できました。

## Q＆A
- Q: なぜ、拡張子を`csv`ではなく`tsv`を選択しているのか？
    - A: `csv`のカラムの区切りが`,`であるため<br>リスト型のデータをCSVに記入したい時に**相性が悪いため**
    ```python:sample_list.py
    sample_list = [1,2,3,4,5]
    ```
- Q: この方法に**デメリット**はないのか？
    - A: あります!
    1. そもそもこの方法を確立するのに**時間がかかる**
    2. 条件が細かいテストケースの場合は、TSVの条件の**書き方を工夫しないといけない**
    3. CSV/TSVファイルから読み取った値は、**全て文字列になる**ため適したデータ型に変換するに苦労する
    4. テストの共通関数を作成するのが、テスト内容によっては**大変である**
    5. テスト対象が変わるたびにテストの共通関数を**新しく準備する必要がある**

## おわりに
当時は、自分を中心にこの企画を進めており、
無事に使えるレベルものになり、チームに導入する事が出来ました。

さきほどのデメリットにも書かせて頂きましたが、
確立するまでに時間がかかる為、
複雑な試験の場合は、期限までに間に合わないと判断し、使わない時もありました。

しかし、テスト条件が簡単でテストパターンが多い試験に相性が良く
想定工数より少なく済むことが出来ました。

一長一短の方法ですが、
当時のチーム内では、以前から求められていた方法だったので
実装したときは、称賛の声を頂きました。
