---
title: "5-1. 共通テーブル式 (CTE)"
weight: 51
---

**共通テーブル式 (Common Table Expression, CTE)** は、複雑なサブクエリを分割し、クエリの可読性を向上させるための機能です。`WITH`句を使い、クエリの前に一時的な名前付きの結果セットを定義できます。

### 構文
{{< highlight sql >}}
WITH cte名 AS (
    -- ここにサブクエリを記述
    SELECT ...
)
SELECT * FROM cte名;
{{< /highlight >}}

### 使用例：部署ごとの従業員数を表示 (サブクエリ版との比較)
第3章のサブクエリの例をCTEで書き換えてみましょう。
{{< highlight sql >}}
WITH emp_counts AS (
    SELECT
        department_id,
        COUNT(*) AS employee_count
    FROM
        employees
    GROUP BY
        department_id
)
SELECT
    d.department_name,
    ec.employee_count
FROM
    departments AS d
INNER JOIN
    emp_counts AS ec ON d.department_id = ec.department_id;
{{< /highlight >}}
このように、処理のステップが上から下に流れるように記述できるため、可読性が大幅に向上します。
