---
title: '条件を満たすデータを取得する'
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

## 基本構文

:::message
下記が`基本構文`なります

```sql
SELECT * FROM テーブル名 WHERE 条件式
```

[記号の種類](https://zenn.dev/aew2sbee/books/basic-postgresql/viewer/free-trivia#%E8%A8%98%E5%8F%B7%E3%81%AE%E7%A8%AE%E9%A1%9E)

[条件式の優先順位](https://zenn.dev/aew2sbee/books/basic-postgresql/viewer/free-trivia#%E8%A8%98%E5%8F%B7%E3%81%AE%E7%A8%AE%E9%A1%9E)

:::

## サンプル

`item_name = 'リンゴ'`に該当する値を取得する

```sql
SELECT * FROM sales WHERE item_name = 'リンゴ'
```

## 出力結果

![select-where](/images/books/basic-postgresql/select-where.png)
