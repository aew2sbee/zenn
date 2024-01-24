---
title: 'はじめに'
free: true
---

## はじめに

2024/01月から`SQL`の学習を開始しました。
アウトプットとして本書を作成します。

他の人にも活用できたら幸いです。
<!-- 
## 前提条件
sales
| id | date | item_name | item_cont | price | customer | discount | discount_rate |
|---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- |
| 1 | 2024-01-01 | リンゴ | 3 | 260 | Aさん | True | 0.1 |
| 2 | 2024-01-02 | バナナ | 2 | 340 | Bさん | False | null |
| 3 | 2024-01-03 | イチゴ | 4 | 550 | Cさん | False | null |
| 4 | 2024-01-04 | リンゴ | 5 | 670 | Dさん | False | null |
| 5 | 2024-01-05 | バナナ | 1 | 560 | Eさん | False | null |
| 6 | 2024-01-06 | バナナ | 4 | 340 | Fさん | True | 0.5 |
| 7 | 2024-01-07 | イチゴ | 6 | 230 | Gさん | False | null |
| 8 | 2024-01-08 | リンゴ | 2 | 410 | Hさん | True | 0.3 | -->

<!-- 
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
``` -->

## さいごに

随時記事を更新します。
