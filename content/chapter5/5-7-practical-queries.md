---
title: "5-7. 実践的クエリテクニック"
weight: 57
---

実務でよく遭遇するデータ形式の変換テクニックを2つ紹介します。

### 横持ちから縦持ちへの変換
`quarterly_sales`テーブルのように、各項目が列になっている「横持ち」データを、分析しやすい「縦持ち」データに変換します。`UNION ALL`を使うのが一般的です。
{{< highlight sql >}}
SELECT product_id, year, 'Q1' AS quarter, q1_sales AS sales FROM quarterly_sales
UNION ALL
SELECT product_id, year, 'Q2' AS quarter, q2_sales AS sales FROM quarterly_sales
UNION ALL
SELECT product_id, year, 'Q3' AS quarter, q3_sales AS sales FROM quarterly_sales
UNION ALL
SELECT product_id, year, 'Q4' AS quarter, q4_sales AS sales FROM quarterly_sales;
{{< /highlight >}}

### 縦持ちから横持ちへの変換（ピボット）
`CASE`式と集約関数を組み合わせることで、行のデータを列に変換（ピボット）できます。
- **顧客ごと、商品カテゴリごとの購入数量マトリクスを作成する**
{{< highlight sql >}}
SELECT
    c.customer_name,
    SUM(CASE WHEN p.category = 'PC' THEN s.quantity ELSE 0 END) AS "PC",
    SUM(CASE WHEN p.category = '周辺機器' THEN s.quantity ELSE 0 END) AS "周辺機器",
    SUM(CASE WHEN p.category = 'ソフトウェア' THEN s.quantity ELSE 0 END) AS "ソフトウェア"
FROM
    sales AS s
JOIN
    customers AS c ON s.customer_id = c.customer_id
JOIN
    products AS p ON s.product_id = p.product_id
GROUP BY
    c.customer_name;
{{< /highlight >}}
