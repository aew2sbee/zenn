---
title: '[SQL] 私なりのチートシート' # 記事のタイトル
emoji: '🫦' # アイキャッチとして使われる絵文字（1文字だけ）
type: 'tech' # tech: 技術記事 / idea: アイデア
topics: ['sql', '初心者向け'] # タグ。["markdown", "rust", "aws"]のように指定する
published: false # 公開設定（falseにすると下書き）
---

## はじめに

SQLの学習をこれまで行っておらず、
PJでBackendの処理を触る機会がある為、`SQL`を下記書籍で学習しました。
チートシートを作成していつでも振り返れる事が出来るようにするために執筆しました。
@[card](https://www.shoeisha.co.jp/book/detail/9784798179612)

## 前提条件

`items`というテーブルに下記のデータが保存しております。
| id |
| ---- |
| 1 |
| 2 |
| 3 |

`sales`というテーブルに下記のデータが保存しております。

| id  | date       | item_name | item_cont | price | customer | discount | discount_rate |
| --- | ---------- | --------- | --------- | ----- | -------- | -------- | ------------- |
| 1   | 2024-01-01 | リンゴ    | 3         | 260   | A さん   | True     | 0.1           |
| 2   | 2024-01-02 | バナナ    | 2         | 340   | B さん   | False    | null          |
| 3   | 2024-01-03 | イチゴ    | 4         | 550   | C さん   | False    | null          |
| 4   | 2024-01-04 | リンゴ    | 5         | 670   | D さん   | False    | null          |
| 5   | 2024-01-05 | バナナ    | 1         | 560   | E さん   | False    | null          |
| 6   | 2024-01-06 | バナナ    | 4         | 340   | F さん   | True     | 0.5           |
| 7   | 2024-01-07 | イチゴ    | 6         | 230   | G さん   | False    | null          |
| 8   | 2024-01-08 | リンゴ    | 2         | 410   | H さん   | True     | 0.3           |

## CREATE

### テーブルを作成する

#### 基本構文

```sql
CREATE TABLE テーブル名 (
    カラム名A データ型,
    カラム名B データ型,
    カラム名C データ型,
    ...
)
```

#### サンプル

sales を下記データ型で設定する

```sql
CREATE TABLE sales (
    id SERIAL PRIMARY KEY,
    date DATE,
    item_name VARCHAR(50),
    item_cont INT,
    price INT,
    customer VARCHAR(50),
    discount BOOLEAN,
    discount_rate REAL
);
```

## DELETE

### 全てデータを削除する

#### 基本構文

```sql
DELETE FROM テーブル名
```

#### サンプル

sales のテーブルの全データを削除する

```sql
DELETE FROM sales WHERE
```
---
### データを削除する

#### 基本構文

```sql
DELETE FROM テーブル名 WHERE 条件式
```

#### サンプル

2024-01-01 のデータを削除する

```sql
DELETE FROM sales WHERE date = '2024-01-01'
```

## DROP

### テーブルを削除する

#### 基本構文

```sql
DROP TABLE テーブル名
```

#### サンプル

sales のテーブルを削除する

```sql
DROP TABLE sales
```

## INSERT

### データを作成する

#### 基本構文

```sql
INSERT INTO テーブル名 VALUES
    (カラム名Aのデータ1, カラム名Bのデータ1, カラム名Cのデータ1),
    (カラム名Aのデータ2, カラム名Bのデータ2, カラム名Cのデータ2),
    (カラム名Aのデータ3, カラム名Bのデータ3, カラム名Cのデータ3),
```

#### サンプル

sales に 8 件のデータを作成する

```sql
INSERT INTO sales VALUES
    (1, '2024-01-01', 'リンゴ', 3, 260, 'Aさん', TRUE, 0.1),
    (2, '2024-01-02', 'バナナ', 2, 340, 'Bさん', FALSE, NULL),
    (3, '2024-01-03', 'イチゴ', 4, 550, 'Cさん', FALSE, NULL),
    (4, '2024-01-04', 'リンゴ', 5, 670, 'Dさん', FALSE, NULL),
    (5, '2024-01-05', 'バナナ', 1, 560, 'Eさん', FALSE, NULL),
    (6, '2024-01-06', 'バナナ', 4, 340, 'Fさん', TRUE, 0.5),
    (7, '2024-01-07', 'イチゴ', 6, 230, 'Gさん', FALSE, NULL),
    (8, '2024-01-08', 'リンゴ', 2, 410, 'Hさん', TRUE, 0.3);
```

## JOIN

### テーブルを結合してデータを取得する

#### 基本構文

```sql
SELECT カラム FROM テーブル名A
INNER JOIN テーブル名B ON テーブル名A.カラム名 = テーブル名B.カラム名
```

#### サンプル

sales と items の id が同じの items のデータを取得する

```sql
SELECT item_name FROM sales
INNER JOIN items ON sales.id = items.id
```
---
### 条件を含めてテーブルを結合してデータを取得する

#### 基本構文

```sql
SELECT カラム FROM テーブル名A
INNER JOIN テーブル名B ON テーブル名A.カラム名 = テーブル名B.カラム名
WHERE 条件式
```

#### サンプル

sales と items の id が同じの items のデータの中で item_cont が 2 より多いデータを取得する

```sql
SELECT item_name FROM sales
INNER JOIN items ON sales.id = items.id
WHERE item_cont > 2
```

## UPDATE

### データを更新する

#### 基本構文

```sql
UPDATE テーブル名 SET カラム名 = 設定したい値 WHERE 条件式
```

#### サンプル

2024-01-01 のデータを 2024-01-15 に更新する

```sql
UPDATE sales SET date = '2024-01-15' WHERE date = '2024-01-01'
```

## SELECT

### 全データを取得する

#### 基本構文

```sql
SELECT * FROM テーブル名
```

#### サンプル

sales の全てのカラムの値を取得する

```sql
SELECT * FROM sales
```

### AND 条件を満たすデータを取得する

#### 基本構文

```sql
SELECT * FROM テーブル名 WHERE 条件式1 AND 条件式2
```

#### サンプル

item_name = `リンゴ`かつ`2024-01-04`以降に該当する値を取得する

```sql
SELECT * FROM sales WHERE item_name = 'リンゴ' AND date >= '2024-01-04'
```

### カラムを別名にしてデータを取得する

### 全データを取得する

#### 基本構文

```sql
SELECT カラム名 AS 別カラム名 FROM テーブル名
```

#### サンプル

item_name = `リンゴ`かつ`2024-01-04`以降に該当する値を取得する

```sql
SELECT item_name AS 商品名 FROM sales
```

### データの平均値を取得する

#### 基本構文

```sql
SELECT AVG(カラム名) FROM テーブル名
```

#### サンプル

price の平均値を取得する

```sql
SELECT AVG(price) FROM sales
```

### 特定の範囲を満たすデータを取得する

#### 基本構文

```sql
SELECT * FROM テーブル名 WHERE カラム名 BETWEEN 条件A AND 条件B
```

#### サンプル

price が 300 と 400 の間に該当する値を取得する

```sql
SELECT * FROM sales WHERE price BETWEEN 300 AND 400
```

### 計算した値を取得する

#### 基本構文

```sql
SELECT 計算 FROM テーブル名
```

#### サンプル

item_name x price の値を取得する

```sql
SELECT item_cont * price as 合計値段 FROM sales
```

### データ数を取得する

#### 基本構文

```sql
SELECT COUNT(*) FROM テーブル名
```

#### サンプル

sales のデータ数を取得する

```sql
SELECT COUNT(*) FROM sales
```

### 重複を取り除いたデータを取得する

#### 基本構文

```sql
SELECT DISTINCT カラム名 FROM テーブル名
```

#### サンプル

item_name の値を取得し重複を取り除く

```sql
SELECT DISTINCT item_name FROM sales
```

### グループ化する条件を加えてデータを取得する

#### 基本構文

```sql
SELECT カラム名A, 集計関数 FROM テーブル名 GROUP BY カラム名A HAVING 条件式
```

#### サンプル

各 item_name のデータ数で 3 回以上のデータを取得する

```sql
SELECT item_name, COUNT(*) FROM sales GROUP BY item_name HAVING COUNT(*) >= 3
```

### データをグループ化して取得する

#### 基本構文

```sql
SELECT カラム名A, 関数(引数) FROM テーブル名 GROUP BY カラム名A
```

#### サンプル

各 item_name のデータ数を取得する

```sql
SELECT item_name, COUNT(*) FROM sales GROUP BY item_name
```

### IN 条件を満たすデータを取得する

#### 基本構文

```sql
SELECT * FROM テーブル名 WHERE カラム名 IN (データ名1, データ名2, ...)
```

#### サンプル

item_name で`リンゴ`, `バナナ`に該当する値を取得する

```sql
SELECT * FROM sales WHERE item_name IN ('リンゴ', 'バナナ')
```

### データの最大値を取得する

#### 基本構文

```sql
SELECT MAX(カラム名) FROM テーブル名
```

#### サンプル

price の最大値を取得する

```sql
SELECT MAX(price) FROM sales
```

### データの最低値を取得する

#### 基本構文

```sql
SELECT MIN(カラム名) FROM テーブル名
```

#### サンプル

price の最低値を取得する

```sql
SELECT MIN(price) FROM sales
```

### 複数カラムのデータを取得する

#### 基本構文

```sql
SELECT カラム名1, カラム名2, ... FROM テーブル名
```

#### サンプル

item_name と customer の値を取得する

```sql
SELECT item_name, customer FROM sales
```

### 特定の範囲を満さないデータを取得する

#### 基本構文

```sql
SELECT * FROM テーブル名 WHERE カラム名 NOt BETWEEN 条件A AND 条件B
```

#### サンプル

price が 300 と 400 の間に該当しない値を取得する

```sql
SELECT * FROM sales WHERE price NOt BETWEEN 300 AND 400
```

### NOT IN 条件を満たすデータを取得する

#### 基本構文

```sql
SELECT * FROM テーブル名 WHERE カラム名 NOT IN (データ名1, データ名2, ...)
```

#### サンプル

item_name で`リンゴ`, `バナナ`に該当しない値を取得する

```sql
SELECT * FROM sales WHERE item_name NOT IN ('リンゴ', 'バナナ')
```

### 条件を満たさないデータを取得する

#### 基本構文

```sql
SELECT * FROM テーブル名 WHERE NOT 条件式
```

#### サンプル

item_name = `リンゴ`に該当しない値を取得する

```sql
SELECT * FROM sales WHERE NOT item_name = 'リンゴ'
```

### OR 条件を満たすデータを取得する

#### 基本構文

```sql
SELECT * FROM テーブル名 WHERE 条件式1 OR 条件式2
```

#### サンプル

item_name = `リンゴ`か item_name != バナナに該当する値を取得する

```sql
SELECT * FROM sales WHERE item_name = 'リンゴ' OR item_name <> 'バナナ'
```

### 降順にデータを並べ替える

#### 基本構文

```sql
SELECT * FROM テーブル名 ORDER BY 並べ替え基準のカラム名 DESC
```

#### サンプル

price が高い順に値を取得する

```sql
SELECT * FROM sales ORDER BY price DESC
```

### 昇順にデータを並べ替える

#### 基本構文

```sql
SELECT * FROM テーブル名 ORDER BY 並べ替え基準のカラム名 ASC
```

#### サンプル

price が安い順に値を取得する

```sql
SELECT * FROM sales ORDER BY price ASC
```

### 複数の条件でデータを並べ替える

#### 基本構文

```sql
SELECT * FROM テーブル名 ORDER BY 並べ替え基準のカラム名A, 並べ替え基準のカラム名B, ...
```

#### サンプル

price が安い順に並べた後に item_cont の小さい順に値を取得する

```sql
SELECT * FROM sales ORDER BY price, item_cont
```

### データの合計値を取得する

#### 基本構文

```sql
SELECT SUM(カラム名) FROM テーブル名
```

#### サンプル

price の合計値を取得する

```sql
SELECT SUM(price) FROM sales
```

### 条件を満たすデータを取得する

#### 基本構文

```sql
SELECT SUM(カラム名) FROM テーブル名
```

#### サンプル

item_name = 'リンゴ'に該当する値を取得する

```sql
SELECT * FROM sales WHERE item_name = 'リンゴ'
```

## 備考

### 記号の種類

| 記号 | 意味                 |
| ---- | -------------------- |
| =    | 左辺と右辺が同じ     |
| <    | 左辺が右辺より小さい |
| >    | 左辺が右辺より大きい |
| <=   | 左辺が右辺以下       |
| =    | 左辺が右辺以上       |
| <>   | 左辺と右辺が異なる   |

### 算術演算子

| 演算子 | 意味                 |
| ------ | -------------------- |
| +      | 足し算               |
| –      | 引き算               |
| *     | 掛け算               |
| /      | 割り算               |
| %      | 割り算の余りを求める |

### SQL 実行順番

| 優先順位 | 説明                               | 句                   |
| -------- | ---------------------------------- | -------------------- |
| 1        | データを取り出すテーブルを指定する | –                    |
| 2        | テーブルを結合す                   | INNER JOIN 句, ON 句 |
| 3        | 取り出すデータの条件を付ける       | WHERE 句             |
| 4        | グループ化                         | GROUP BY 句          |
| 5        | グループ化した後に条件を付ける     | HAVING 句            |
| 6        | 取り出すカラムを指定する           | SELECT 句            |
| 7        | レコードを並べ替える               | ORDER BY 句          |
