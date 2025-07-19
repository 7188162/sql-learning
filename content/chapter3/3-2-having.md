---
title: "3-2. 集計結果の絞り込み (HAVING)"
weight: 32
---
`GROUP BY`で集計した**後**の結果に対して、さらに条件を指定したい場合は`HAVING`句を使います。

- **従業員が2人以上いる部署だけを表示する**
  {{< highlight sql >}}
  SELECT
      department_id,
      COUNT(*) AS employee_count
  FROM
      employees
  GROUP BY
      department_id
  HAVING
      COUNT(*) >= 2;
  {{< /highlight >}}

{{< infobox title="コラム: WHERE と HAVING はどこが違う？" >}}
`WHERE`も`HAVING`も条件を指定する点は同じですが、 **処理されるタイミング**が決定的に違います。
- **`WHERE`句:** `GROUP BY`で集計される**前**に、元のテーブルの各行に対して適用されます。
- **`HAVING`句:** `GROUP BY`で集計された**後**の結果に対して適用されます。

そのため、`WHERE`句では集約関数を使えませんが、`HAVING`句では使えます。
SQLの内部的な処理順序は、大まかに `FROM` -> `WHERE` -> `GROUP BY` -> `HAVING` -> `SELECT` -> `ORDER BY` となっています。
{{< /infobox >}}
