---
title: '[JOIN] 条件を含めてテーブルを結合してデータを取得する'
free: true
---

## 前提条件


`items`というテーブルに下記のデータが保存しております。
| id |
|---- |
| 1 |
| 2 |
| 3 |

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


## 基本構文

:::message
下記が`基本構文`なります

```sql
SELECT カラム FROM テーブル名A
INNER JOIN テーブル名B ON テーブル名A.カラム名 = テーブル名B.カラム名
WHERE 条件式
```

:::

### サンプル
`sales`と`items`の`id`が同じの`items`のデータの中で`item_cont`が2より多いデータを取得する

```sql
SELECT item_name FROM sales
INNER JOIN items ON sales.id = items.id
WHERE item_cont > 2
```


### 出力結果

![join-where](/images/books/basic-postgresql/join-where.png)
