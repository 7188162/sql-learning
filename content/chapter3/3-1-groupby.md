---
title: "3-1. グループ化と集計"
weight: 31
---
データを特定の条件でグループに分け、それぞれのグループに対して合計や平均などの計算を行うのが「集計」です。`GROUP BY`句と「集約関数」を使います。

### 集約関数
集約を行うための関数を「集約関数」と呼びます。代表的なものは以下の通りです。

| 関数 | 説明 |
|:---|:---|
| `COUNT(カラム名 or *)` | 行の数を数える。`COUNT(*)`は`NULL`を含めた全行を数える。 |
| `SUM(カラム名)` | 数値カラムの合計値を計算する。 |
| `AVG(カラム名)` | 数値カラムの平均値を計算する。 |
| `MAX(カラム名)` | カラムの最大値を取得する。 |
| `MIN(カラム名)` | カラムの最小値を取得する。 |

### データをグループ化する (`GROUP BY`)
`GROUP BY`句を使うと、指定したカラムの値が同じ行を1つのグループとしてまとめ、そのグループごとに集約関数を適用できます。

- **部署ごとの従業員数を数える**
  `departments`テーブルの`department_id`ごとにグループ化し、それぞれの人数を`COUNT`で数えます。
  {{< highlight sql >}}
  SELECT
      department_id,
      COUNT(*) AS employee_count
  FROM
      employees
  WHERE
      department_id IS NOT NULL -- 部署未所属の従業員は除外
  GROUP BY
      department_id;
  {{< /highlight >}}

- **商品カテゴリごとの平均価格を求める**
  {{< highlight sql >}}
  SELECT
      category,
      AVG(price) AS average_price
  FROM
      products
  GROUP BY
      category;
  {{< /highlight >}}

{{< infobox title="GROUP BY の重要ルール" >}}
`GROUP BY`を使った`SELECT`文では、`SELECT`句に書けるのは以下の2種類だけです。
1. `GROUP BY`で指定したカラム
2. 集約関数（`COUNT`, `SUM`など）
これ以外のカラムを書くと、どの行の値を表示すればいいかデータベースが判断できないため、エラーになります。
{{< /infobox >}}
