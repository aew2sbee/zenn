---
title: '[SELECT文] データを取得する'
free: true
---

## 前提条件

`sales`というテーブルに下記のデータが保存しております。
| id | date | item_name | item_cont | price | customer | discount | discount_rate |
|---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- |
| 1 | 2024-01-01 | リンゴ | 3 | 260 | A さん | True | 0.1 |
| 2 | 2024-01-02 | バナナ | 2 | 340 | B さん | False | null |
| 3 | 2024-01-03 | イチゴ | 4 | 550 | C さん | False | null |
| 4 | 2024-01-04 | リンゴ | 5 | 670 | D さん | False | null |
| 5 | 2024-01-05 | バナナ | 1 | 560 | E さん | False | null |
| 6 | 2024-01-06 | バナナ | 4 | 340 | F さん | True | 0.5 |
| 7 | 2024-01-07 | イチゴ | 6 | 230 | G さん | False | null |
| 8 | 2024-01-08 | リンゴ | 2 | 410 | H さん | True | 0.3 |

## 1. 全データを取得する

:::message
下記が`基本構文`なります

```sql
SELECT * FROM テーブル名
```

:::

### サンプル

`sales`の全てのカラムの値を取得する

```sql
SELECT * FROM sales
```

### 出力結果

![SELECT-01](/images/books/basic-postgresql/SELECT-01.png)

## 2. 指定カラムのデータを取得する

:::message
下記が`基本構文`なります

```sql
SELECT カラム名 FROM テーブル名
```

:::

### サンプル

`item_name`の値を取得する

```sql
SELECT item_name FROM sales
```

### 出力結果

![SELECT-02](/images/books/basic-postgresql/SELECT-02.png)

## 3. 複数カラムのデータを取得する

:::message
下記が`基本構文`なります

```sql
SELECT DISTINCT カラム名1, カラム名2, ... FROM テーブル名
```

:::

### サンプル

`item_name`と`customer`の値を取得する

```sql
SELECT item_name, customer FROM sales
```

### 出力結果

![SELECT-03](/images/books/basic-postgresql/SELECT-03.png)

## 4. 重複を取り除いたデータを取得する

:::message
下記が`基本構文`なります

```sql
SELECT DISTINCT カラム名 FROM テーブル名
```

:::

### サンプル

`item_name`の値を取得し重複を取り除く

```sql
SELECT DISTINCT item_name FROM sales
```

### 出力結果

![SELECT-04](/images/books/basic-postgresql/SELECT-04.png)

## 5. 指定したカラムを別名にしてデータを取得する

:::message
下記が`基本構文`なります

```sql
SELECT カラム名 AS 別カラム名 FROM テーブル名
```

:::

### サンプル

`item_name` -> `商品名`として値を取得する

```sql
SELECT item_name AS 商品名 FROM sales
```

### 出力結果

![SELECT-05](/images/books/basic-postgresql/SELECT-05.png)

## 6. 指定した条件を満たすデータを取得する

:::message
下記が`基本構文`なります

```sql
SELECT * FROM テーブル名 WHERE 条件式
```

**記号の種類**

| 記号 | 意味                 |
| ---- | -------------------- |
| =    | 左辺と右辺が同じ     |
| <    | 左辺が右辺より小さい |
| >    | 左辺が右辺より大きい |
| <=   | 左辺が右辺以下       |
| >=   | 左辺が右辺以上       |
| <>   | 左辺と右辺が異なる   |

:::

### サンプル

`item_name = 'リンゴ'`に該当する値を取得する

```sql
SELECT * FROM sales WHERE item_name = 'リンゴ'
```

### 出力結果

![SELECT-06](/images/books/basic-postgresql/SELECT-06.png)

## 7. 複数条件を満たすデータを取得する

:::message
下記が`基本構文`なります

```sql
SELECT * FROM テーブル名 WHERE 条件式1 AND 条件式2
```

:::

### サンプル

`item_name = 'リンゴ'`かつ`'2024-01-04'以降`に該当する値を取得する

```sql
SELECT * FROM sales WHERE item_name = 'リンゴ' AND date >= '2024-01-04'
```

### 出力結果

![SELECT-07](/images/books/basic-postgresql/SELECT-07.png)

## 8. 複数条件を満たすデータを取得する

:::message
下記が`基本構文`なります

```sql
SELECT * FROM テーブル名 WHERE 条件式1 OR 条件式2
```

:::

### サンプル

`item_name = 'リンゴ'`か`'item_name != バナナ`に該当する値を取得する

```sql
SELECT * FROM sales WHERE item_name = 'リンゴ' OR item_name <> 'バナナ'
```

### 出力結果

![SELECT-08](/images/books/basic-postgresql/SELECT-08.png)