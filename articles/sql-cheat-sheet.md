---
title: "[SQL] 私なりのチートシート" # 記事のタイトル
emoji: "🗃️" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["sql", "初心者向け"] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開:true / 非公開:false
---

## はじめに

この記事では、**基本的な SQL コマンド**を解説します。

:::details 参考資料
@[card](https://www.shoeisha.co.jp/book/detail/9784798179612)
:::

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

### 1. テーブルを作成する

```sql:基本構文
CREATE TABLE テーブル名 (
    カラム名A データ型,
    カラム名B データ型,
    カラム名C データ型,
    ...
)
```

```sql:サンプル
-- sales を下記データ型で設定する
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

### 1. 全てデータを削除する

```sql:基本構文
DELETE FROM テーブル名
```

```sql:サンプル
-- sales のテーブルの全データを削除する
DELETE FROM sales WHERE
```

---

### 2. データを削除する

```sql:基本構文
DELETE FROM テーブル名 WHERE 条件式
```

```sql:サンプル
2024-01-01 のデータを削除する
DELETE FROM sales WHERE date = '2024-01-01'
```

## DROP

### 1. テーブルを削除する

```sql: 基本構文
DROP TABLE テーブル名
```

```sql: サンプル
-- sales のテーブルを削除する
DROP TABLE sales
```

## INSERT

### 1. データを作成する

```sql:基本構文
INSERT INTO テーブル名 VALUES
    (カラム名Aのデータ1, カラム名Bのデータ1, カラム名Cのデータ1),
    (カラム名Aのデータ2, カラム名Bのデータ2, カラム名Cのデータ2),
    (カラム名Aのデータ3, カラム名Bのデータ3, カラム名Cのデータ3),
```

```sql: サンプル
-- sales に 8 件のデータを作成する
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

### 1. テーブルを結合してデータを取得する

```sql: 基本構文
SELECT カラム FROM テーブル名A
INNER JOIN テーブル名B ON テーブル名A.カラム名 = テーブル名B.カラム名
```

```sql:サンプル
-- sales と items の id が同じの items のデータを取得する
SELECT item_name FROM sales
INNER JOIN items ON sales.id = items.id
```

---

### 2. 条件を含めてテーブルを結合してデータを取得する

```sql:基本構文
SELECT カラム FROM テーブル名A
INNER JOIN テーブル名B ON テーブル名A.カラム名 = テーブル名B.カラム名
WHERE 条件式
```

```sql:サンプル
-- sales と items の id が同じの items のデータの中で item_cont が 2 より多いデータを取得する
SELECT item_name FROM sales
INNER JOIN items ON sales.id = items.id
WHERE item_cont > 2
```

## UPDATE

### 1. データを更新する

```sql: 基本構文
UPDATE テーブル名 SET カラム名 = 設定したい値 WHERE 条件式
```

```sql: サンプル
-- 2024-01-01 のデータを 2024-01-15 に更新する
UPDATE sales SET date = '2024-01-15' WHERE date = '2024-01-01'
```

## SELECT

### 1. 全データを取得する

```sql:基本構文
SELECT * FROM テーブル名
```

```sql:サンプル
-- sales の全てのカラムの値を取得する
SELECT * FROM sales
```

### 2. AND 条件を満たすデータを取得する

```sql:基本構文
SELECT * FROM テーブル名 WHERE 条件式1 AND 条件式2
```

```sql:サンプル
-- item_name = `リンゴ`かつ`2024-01-04`以降に該当する値を取得する
SELECT * FROM sales WHERE item_name = 'リンゴ' AND date >= '2024-01-04'
```

### 3. カラムを別名にしてデータを取得する

```sql: 基本構文
SELECT カラム名 AS 別カラム名 FROM テーブル名
```

```sql:サンプル
-- item_name -> 商品名として値を取得する
SELECT item_name AS 商品名 FROM sales
```

### 4. データの平均値を取得する

```sql:基本構文
SELECT AVG(カラム名) FROM テーブル名
```

```sql:サンプル
-- price の平均値を取得する
SELECT AVG(price) FROM sales
```

### 5. 特定の範囲を満たすデータを取得する

```sql:基本構文
SELECT * FROM テーブル名 WHERE カラム名 BETWEEN 条件A AND 条件B
```

```sql:サンプル
-- price が 300 と 400 の間に該当する値を取得する
SELECT * FROM sales WHERE price BETWEEN 300 AND 400
```

### 6. 計算した値を取得する

```sql:基本構文
SELECT 計算 FROM テーブル名
```

```sql:サンプル
-- item_name と price の乗算を取得する
SELECT item_cont * price as 合計値段 FROM sales
```

### 7. データ数を取得する

```sql:基本構文
SELECT COUNT(*) FROM テーブル名
```

```sql:サンプル
-- sales のデータ数を取得する
SELECT COUNT(*) FROM sales
```

### 8. 重複を取り除いたデータを取得する

```sql:基本構文
SELECT DISTINCT カラム名 FROM テーブル名
```

```sql:サンプル
-- item_name の値を取得し重複を取り除く
SELECT DISTINCT item_name FROM sales
```

### 9. グループ化する条件を加えてデータを取得する

```sql:基本構文
SELECT カラム名A, 集計関数 FROM テーブル名 GROUP BY カラム名A HAVING 条件式
```

```sql:サンプル
-- 各 item_name のデータ数で 3 回以上のデータを取得する
SELECT item_name, COUNT(*) FROM sales GROUP BY item_name HAVING COUNT(*) >= 3
```

### 10. データをグループ化して取得する

```sql:基本構文
SELECT カラム名A, 関数(引数) FROM テーブル名 GROUP BY カラム名A
```

```sql:サンプル
-- 各 item_name のデータ数を取得する
SELECT item_name, COUNT(*) FROM sales GROUP BY item_name
```

### 11. IN 条件を満たすデータを取得する

```sql:基本構文
SELECT * FROM テーブル名 WHERE カラム名 IN (データ名1, データ名2, ...)
```

```sql:サンプル
-- item_name で`リンゴ`, `バナナ`に該当する値を取得する
SELECT * FROM sales WHERE item_name IN ('リンゴ', 'バナナ')
```

### 12. データの最大値を取得する

```sql:基本構文
SELECT MAX(カラム名) FROM テーブル名
```

```sql:サンプル
-- price の最大値を取得する
SELECT MAX(price) FROM sales
```

### 13. データの最低値を取得する

```sql:基本構文
SELECT MIN(カラム名) FROM テーブル名
```

```sql:サンプル
-- price の最低値を取得する
SELECT MIN(price) FROM sales
```

### 14. 複数カラムのデータを取得する

```sql:基本構文
SELECT カラム名1, カラム名2, ... FROM テーブル名
```

```sql:サンプル
-- item_name と customer の値を取得する
SELECT item_name, customer FROM sales
```

### 15. 特定の範囲を満さないデータを取得する

```sql:基本構文
SELECT * FROM テーブル名 WHERE カラム名 NOt BETWEEN 条件A AND 条件B
```

```sql:サンプル
-- price が 300 と 400 の間に該当しない値を取得する
SELECT * FROM sales WHERE price NOt BETWEEN 300 AND 400
```

### 16. NOT IN 条件を満たすデータを取得する

```sql:基本構文
SELECT * FROM テーブル名 WHERE カラム名 NOT IN (データ名1, データ名2, ...)
```

```sql:サンプル
-- item_name で`リンゴ`, `バナナ`に該当しない値を取得する
SELECT * FROM sales WHERE item_name NOT IN ('リンゴ', 'バナナ')
```

### 17. 条件を満たさないデータを取得する

```sql: 基本構文
SELECT * FROM テーブル名 WHERE NOT 条件式
```

```sql:サンプル
-- item_name = `リンゴ`に該当しない値を取得する
SELECT * FROM sales WHERE NOT item_name = 'リンゴ'
```

### 18. OR 条件を満たすデータを取得する

```sql:基本構文
SELECT * FROM テーブル名 WHERE 条件式1 OR 条件式2
```

```sql:サンプル
-- item_name = `リンゴ`か item_name != バナナに該当する値を取得する
SELECT * FROM sales WHERE item_name = 'リンゴ' OR item_name <> 'バナナ'
```

### 19. 降順にデータを並べ替える

```sql:基本構文
SELECT * FROM テーブル名 ORDER BY 並べ替え基準のカラム名 DESC
```

```sql:サンプル
-- price が高い順に値を取得する
SELECT * FROM sales ORDER BY price DESC
```

### 20. 昇順にデータを並べ替える

```sql: 基本構文
SELECT * FROM テーブル名 ORDER BY 並べ替え基準のカラム名 ASC
```

```sql:サンプル
-- price が安い順に値を取得する
SELECT * FROM sales ORDER BY price ASC
```

### 21.複数の条件でデータを並べ替える

```sql:基本構文
SELECT * FROM テーブル名 ORDER BY 並べ替え基準のカラム名A, 並べ替え基準のカラム名B, ...
```

```sql:サンプル
-- price が安い順に並べた後に item_cont の小さい順に値を取得する
SELECT * FROM sales ORDER BY price, item_cont
```

### 22. データの合計値を取得する

```sql:基本構文
SELECT SUM(カラム名) FROM テーブル名
```

```sql:サンプル
-- price の合計値を取得する
SELECT SUM(price) FROM sales
```

### 23. 条件を満たすデータを取得する

```sql:基本構文
SELECT SUM(カラム名) FROM テーブル名
```

```sql:サンプル
-- item_name = 'リンゴ'に該当する値を取得する
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
| \*     | 掛け算               |
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
