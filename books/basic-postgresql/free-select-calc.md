---
title: '[SELECT] 計算した値を取得する'
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
SELECT 計算 FROM テーブル名
```
[算術演算子について](https://zenn.dev/aew2sbee/books/basic-postgresql/viewer/free-trivia#%E7%AE%97%E8%A1%93%E6%BC%94%E7%AE%97%E5%AD%90)

:::

### サンプル

`item_name`x`price`の値を取得する

```sql
SELECT item_cont * price as 合計値段 FROM sales
```

### 出力結果

![select-calc](/images/books/basic-postgresql/select-calc.png)
