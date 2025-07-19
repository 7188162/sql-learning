---
title: "4-3. 日付/時刻関数"
weight: 43
---
日付や時刻に関する情報を取得したり、計算したりします。この分野もDBMSによる方言が多いです。

### 現在の日付/時刻の取得
| 関数名 | 主な対応DBMS |
|:---|:---|
| `CURRENT_TIMESTAMP` | 標準SQL, PostgreSQL, MySQL |
| `NOW()` | PostgreSQL, MySQL |
| `GETDATE()` | SQL Server |
| `CURRENT DATE` / `CURRENT TIME` | IBM Db2 |
| `DATE('now')`, `DATETIME('now')` | SQLite |

### 日付/時刻から特定部分を抽出
- **`EXTRACT()` (標準SQL, PostgreSQL)**
  {{< highlight sql >}}
  -- `sale_date`から「月」だけを抽出
  SELECT EXTRACT(MONTH FROM sale_date) FROM sales;
  {{< /highlight >}}
- **`YEAR()`, `MONTH()`, `DAY()` など (MySQL)**
  {{< highlight sql >}}
  SELECT MONTH(sale_date) FROM sales;
  {{< /highlight >}}
- **`STRFTIME()` (SQLite)**
  {{< highlight sql >}}
  -- %m は月を意味する
  SELECT STRFTIME('%m', sale_date) FROM sales;
  {{< /highlight >}}
