---
title: "3-4. サブクエリ（副問い合わせ）"
weight: 34
---
サブクエリは、SQL文の中に埋め込まれた別の`SELECT`文のことです。クエリを段階的に複雑に組み立てる際に役立ちます。

- **`WHERE`句で使う例：平均給与より高い給与をもらっている従業員を探す**
  まず平均給与を計算し、その結果を使って従業員を絞り込みます。
  {{< highlight sql >}}
  SELECT employee_name, salary
  FROM employees
  WHERE salary > (SELECT AVG(salary) FROM employees);
  {{< /highlight >}}

- **`FROM`句で使う例：部署ごとの従業員数と部署名を結合する**
  `GROUP BY`で集計した結果（派生テーブル）を、`departments`テーブルと結合します。
  {{< highlight sql >}}
  SELECT
      d.department_name,
      emp_counts.employee_count
  FROM
      departments AS d
  INNER JOIN
      (SELECT department_id, COUNT(*) AS employee_count
       FROM employees
       GROUP BY department_id) AS emp_counts
  ON d.department_id = emp_counts.department_id;
  {{< /highlight >}}

{{< infobox title="コラム：サブクエリの可読性" >}}
サブクエリは便利ですが、入れ子が深くなると非常に読みにくくなるという欠点があります。第5章で学ぶ **共通テーブル式（CTE）** を使うと、このような複雑なクエリをよりシンプルに記述できます。
{{< /infobox >}}
