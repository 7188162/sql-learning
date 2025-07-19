---
title: "4-4. 型変換と条件分岐"
weight: 44
---
`CAST`と`CASE`は、SQLで少し複雑なロジックを組み立てる際に非常に重要です。

### 型を変換する (`CAST`)
`CAST`は、あるデータ型の値を別のデータ型に変換する関数です。
例えば、文字列として保存されている数字（例：'123'）を、計算可能な数値型（INTEGER）に変換する際に使います。

{{< highlight sql >}}
-- '2023-01-01' という文字列をDATE型に変換する例
CAST('2023-01-01' AS DATE)
-- 100 という数値を文字列に変換する例
CAST(100 AS VARCHAR(10))
{{< /highlight >}}

### 条件によって結果を分ける (`CASE`式)
`CASE`式は、SQL文の中でif-then-elseのような条件分岐を実現します。

**`CASE`式の基本構文（検索CASE式）**
{{< highlight sql >}}
CASE
    WHEN 条件1 THEN 結果1
    WHEN 条件2 THEN 結果2
    ...
    ELSE 結果N -- どの条件にも合致しない場合
END
{{< /highlight >}}

**使用例1：価格帯に応じて商品を分類する**
`products`テーブルの価格に応じて、「高価格」「中価格」「通常価格」のラベルを付けます。
{{< highlight sql >}}
SELECT
    product_name,
    price,
    CASE
        WHEN price >= 100000 THEN '高価格'
        WHEN price >= 50000  THEN '中価格'
        ELSE '通常価格'
    END AS price_rank
FROM
    products;
{{< /highlight >}}

**使用例2：`GROUP BY`と組み合わせて条件別の集計を行う**
`customers`テーブルで、関東地方とそれ以外の地域の顧客数を集計します。
このクエリは、`GROUP BY`句で`SELECT`句で定義した別名(`area`)が使えることを示しています。
{{< highlight sql >}}
SELECT
    CASE
        WHEN region = '関東' THEN '関東地方'
        ELSE 'その他'
    END AS area,
    COUNT(*) AS customer_count
FROM
    customers
GROUP BY
    area;
{{< /highlight >}}
