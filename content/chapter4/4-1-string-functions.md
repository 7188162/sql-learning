---
title: "4-1. 文字列関数"
weight: 41
---
テキストデータを整形したり、一部を抜き出したりする際に使用します。

| 関数 | 説明 | 例 (`' SQL '` という文字列に適用) |
|:---|:---|:---|
| `LENGTH(str)` / `CHAR_LENGTH(str)` / `LEN(str)` | 文字列の長さを返す。 | `LENGTH(' SQL ')` -> 5 |
| `UPPER(str)` / `LOWER(str)` | 大文字/小文字に変換する。 | `UPPER(' SQL ')` -> `' SQL '` |
| `SUBSTRING(str FROM pos FOR len)` | 文字列の一部を切り出す。 | `SUBSTRING(' SQL ' FROM 2 FOR 3)` -> `'SQL'` |
| `REPLACE(str, from, to)` | 文字列を置換する。 | `REPLACE(' SQL ', ' ', '_')` -> `'_SQL_'` |
| `TRIM(str)` | 両端の空白を除去する。 | `TRIM(' SQL ')` -> `'SQL'` |

### 文字列の結合
文字列同士を連結します。構文はDBMSによる違いが大きいです。
- **`||` (標準SQL, PostgreSQL, SQLite)**
  {{< highlight sql >}}
  SELECT '顧客名: ' || customer_name FROM customers;
  {{< /highlight >}}
- **`CONCAT()` 関数 (MySQL, PostgreSQL)**
  {{< highlight sql >}}
  SELECT CONCAT('顧客名: ', customer_name) FROM customers;
  {{< /highlight >}}
- **`+` 演算子 (MS Access, SQL Server)**
  {{< highlight sql >}}
  SELECT '顧客名: ' + customer_name FROM customers;
  {{< /highlight >}}
