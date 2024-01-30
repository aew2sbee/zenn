---
title: '[CREATE] テーブルを作成する'
free: true
---

## 基本構文

:::message
下記が`基本構文`なります

```sql
CREATE TABLE テーブル名 (
    カラム名A データ型,
    カラム名B データ型,
    カラム名C データ型,
    ...
)
```

:::

### サンプル

`sales`を下記データ型で設定する

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