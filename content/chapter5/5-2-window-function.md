---
title: "5-2. ウィンドウ関数"
weight: 52
---

ウィンドウ関数は、`GROUP BY`のように行を集約せず、 **元の行を残したまま**で集計や順位付けを行える、データ分析の強力な武器です。`OVER()`句と一緒に使います。

- **`PARTITION BY` **: `GROUP BY`のように、どのグループで計算するかを指定します。
- **`ORDER BY` **: グループ内でどの順序で計算するかを指定します。

### 使用例1：部署内で給与が高い順にランキングを付ける
`ROW_NUMBER()`は、指定された順序で連番を振るウィンドウ関数です。
{{< highlight sql >}}
SELECT
    e.employee_name,
    d.department_name,
    e.salary,
    ROW_NUMBER() OVER(PARTITION BY d.department_id ORDER BY e.salary DESC) AS rank_in_dept
FROM
    employees AS e
JOIN
    departments AS d ON e.department_id = d.department_id;
{{< /highlight >}}

### 使用例2：売上の累積合計を計算する
`SUM() OVER()`で、月ごとの売上と、そこまでの累積売上を同時に計算できます。
{{< highlight sql >}}
WITH monthly_sales AS (
  SELECT
    STRFTIME('%Y-%m', sale_date) AS sales_month,
    SUM(p.price * s.quantity) AS monthly_total
  FROM sales AS s
  JOIN products AS p ON s.product_id = p.product_id
  GROUP BY sales_month
)
SELECT
  sales_month,
  monthly_total,
  SUM(monthly_total) OVER(ORDER BY sales_month) AS cumulative_total
FROM
  monthly_sales;
{{< /highlight >}}

{{< dbmsdiff title="ウィンドウ関数のサポート" >}}
ウィンドウ関数は比較的新しい機能のため、古いバージョンのDBMSや一部の簡易的なDBMSではサポートされていません。

**サポートしている主なDBMS:**
{{< badges names="PostgreSQL, MySQL, Db2, SQLite" >}}
<small>※MySQL, SQLiteは比較的新しいバージョンでのサポートとなります。</small>

**サポートしていない主なDBMS:**
<p>MS Accessではウィンドウ関数は使用できません。</p>
{{< /dbmsdiff >}}
