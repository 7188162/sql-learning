---
title: "A-3. 演習問題の解答・解説"
weight: 83
tags: ["exercise", "answer", "solution"]
---

このページでは、各章の末尾にある演習問題の解答例と、簡単な解説を記載します。

---

## 第1章の解答
<small>[第1章の演習問題に戻る &raquo;](/chapter1/1-5-excercise)</small>

### 問題1
以下の仕様で、書籍情報を管理するための `books` テーブルを作成するSQL文を書いてみましょう。

**解答例 (SQLite版)**
{{< highlight sql >}}
CREATE TABLE books (
    book_id         INTEGER PRIMARY KEY AUTOINCREMENT,
    title           TEXT NOT NULL,
    author          TEXT,
    published_date  TEXT,
    price           INTEGER
);
{{< /highlight >}}
**解説:**
`INTEGER PRIMARY KEY AUTOINCREMENT` は、SQLiteで自動採番の主キーを作成する際の一般的な記述です。`NOT NULL`制約で`title`は必須項目としています。

---

## 第2章の解答
<small>[第2章の演習問題に戻る &raquo;](/chapter2/2-6-excercise)</small>

### 問題1
`products`テーブルから、カテゴリが「周辺機器」で、価格が10,000円以上の商品を検索するSQL文を書いてください。

**解答例**
{{< highlight sql >}}
SELECT *
FROM products
WHERE category = '周辺機器' AND price >= 10000;
{{< /highlight >}}
**解説:**
`WHERE`句に`AND`を使って2つの条件を結合しています。

### 問題2, 3, 4
顧客データの追加、更新、削除。

**解答例 (一連の流れ)**
{{< highlight sql >}}
-- 2. 新しい顧客データを追加
INSERT INTO customers (customer_id, customer_name, region)
VALUES (5, '自分の名前', '九州');

-- 3. 上記で追加したデータのregionを'北海道'に更新
UPDATE customers
SET region = '北海道'
WHERE customer_id = 5;

-- 4. 上記で更新したデータを削除
DELETE FROM customers
WHERE customer_id = 5;
{{< /highlight >}}
**解説:**
`INSERT`, `UPDATE`, `DELETE`の基本的な構文です。特に`UPDATE`と`DELETE`では、操作対象を特定するための`WHERE`句が非常に重要です。

---

## 第3章の解答
<small>[第3章の演習問題に戻る &raquo;](/chapter3/3-5-excercise)</small>

### 問題1
顧客ごと(`customer_id`)の注文件数を求めてください。

**解答例**
{{< highlight sql >}}
SELECT
    customer_id,
    COUNT(sale_id) AS order_count
FROM
    sales
GROUP BY
    customer_id;
{{< /highlight >}}

### 問題2
カテゴリごとの商品の最高価格と最低価格を求めてください。

**解答例**
{{< highlight sql >}}
SELECT
    category,
    MAX(price) AS max_price,
    MIN(price) AS min_price
FROM
    products
GROUP BY
    category;
{{< /highlight >}}

### 問題3, 4
商品ごとの販売数量合計を求め、合計が5個以上の商品だけを表示してください。

**解答例**
{{< highlight sql >}}
SELECT
    p.product_name,
    SUM(s.quantity) AS total_quantity
FROM
    sales AS s
JOIN
    products AS p ON s.product_id = p.product_id
GROUP BY
    p.product_name
HAVING
    SUM(s.quantity) >= 5;
{{< /highlight >}}
**解説:**
`GROUP BY`で集計した後の結果に対して条件を適用するため、`WHERE`ではなく`HAVING`句を使用します。

### 問題5
全ての部署名と、そこに所属する従業員名を表示してください。従業員が一人もいない部署も表示されるようにしてください。

**解答例**
{{< highlight sql >}}
SELECT
    d.department_name,
    e.employee_name
FROM
    departments AS d
LEFT JOIN
    employees AS e ON d.department_id = e.department_id;
{{< /highlight >}}
**解説:**
部署テーブル(`departments`)を主軸にし、従業員がいない部署も結果に残すため、`LEFT JOIN`を使用します。

---

## 第4章の解答
<small>[第4章の演習問題に戻る &raquo;](/chapter4/4-5-excercise)</small>

### 問題1
従業員名の後ろに`(ID: [employee_id])`という文字列を連結して表示してください。

**解答例 (SQLite版)**
{{< highlight sql >}}
SELECT
    employee_name || '(ID: ' || employee_id || ')' AS employee_with_id
FROM
    employees;
{{< /highlight >}}

### 問題2
`sale_date`から「年」だけを抽出してください。

**解答例 (SQLite版)**
{{< highlight sql >}}
SELECT
    sale_date,
    STRFTIME('%Y', sale_date) AS sales_year
FROM
    sales;
{{< /highlight >}}

### 問題3
給与を12倍して年収として表示してください。

**解答例**
{{< highlight sql >}}
SELECT
    employee_name,
    salary,
    salary * 12 AS annual_salary
FROM
    employees;
{{< /highlight >}}

### 問題4
`CASE`式を使い、`category`を日本語に変換してください。

**解答例**
{{< highlight sql >}}
SELECT
    product_name,
    category,
    CASE category
        WHEN 'PC' THEN 'パーソナルコンピュータ'
        WHEN '周辺機器' THEN 'アクセサリ'
        ELSE 'その他'
    END AS category_jp
FROM
    products;
{{< /highlight >}}

---

## 第5章の解答
<small>[第5章の演習問題に戻る &raquo;](/chapter5/5-8-excercise)</small>

### 問題1
CTEを使い、部署平均給与と従業員の給与を比較してください。

**解答例**
{{< highlight sql >}}
WITH dept_avg_salary AS (
    SELECT
        department_id,
        AVG(salary) AS avg_salary
    FROM
        employees
    WHERE
        department_id IS NOT NULL
    GROUP BY
        department_id
)
SELECT
    e.employee_name,
    e.salary,
    d.avg_salary,
    CASE
        WHEN e.salary > d.avg_salary THEN '平均より高い'
        ELSE '平均以下'
    END AS comparison
FROM
    employees AS e
JOIN
    dept_avg_salary AS d ON e.department_id = d.department_id;
{{< /highlight >}}

### 問題2
ウィンドウ関数を使い、全従業員を給与が高い順にランク付けしてください。

**解答例**
{{< highlight sql >}}
SELECT
    employee_name,
    salary,
    RANK() OVER (ORDER BY salary DESC) AS salary_rank
FROM
    employees;
{{< /highlight >}}
**解説:**
`RANK()`は同順位を考慮します。`ROW_NUMBER()`を使うと同順位でも連番になります。

### 問題3
横持ち→縦持ち変換後、年ごとの四半期別合計売上を計算してください。

**解答例**
{{< highlight sql >}}
WITH vertical_sales AS (
    SELECT product_id, year, 'Q1' AS quarter, q1_sales AS sales FROM quarterly_sales
    UNION ALL
    SELECT product_id, year, 'Q2' AS quarter, q2_sales AS sales FROM quarterly_sales
    UNION ALL
    SELECT product_id, year, 'Q3' AS quarter, q3_sales AS sales FROM quarterly_sales
    UNION ALL
    SELECT product_id, year, 'Q4' AS quarter, q4_sales AS sales FROM quarterly_sales
)
SELECT
    year,
    quarter,
    SUM(sales) AS total_quarter_sales
FROM
    vertical_sales
GROUP BY
    year, quarter
ORDER BY
    year, quarter;
{{< /highlight >}}

### 問題4
従業員ごと・顧客ごとの売上合計金額マトリクスを作成してください。

**解答例**
{{< highlight sql >}}
SELECT
    e.employee_name,
    SUM(CASE WHEN c.customer_name = '株式会社ABC' THEN p.price * s.quantity ELSE 0 END) AS "株式会社ABC",
    SUM(CASE WHEN c.customer_name = '株式会社DEF' THEN p.price * s.quantity ELSE 0 END) AS "株式会社DEF",
    SUM(CASE WHEN c.customer_name = 'GHI商事' THEN p.price * s.quantity ELSE 0 END) AS "GHI商事",
    SUM(CASE WHEN c.customer_name = '株式会社JKL' THEN p.price * s.quantity ELSE 0 END) AS "株式会社JKL"
FROM
    sales AS s
JOIN
    employees AS e ON s.employee_id = e.employee_id
JOIN
    customers AS c ON s.customer_id = c.customer_id
JOIN
    products AS p ON s.product_id = p.product_id
GROUP BY
    e.employee_name;
{{< /highlight >}}
**解説:**
`CASE`式を使って、条件に合致する場合のみ売上金額を、合致しない場合は`0`を返すようにし、それを`SUM`で集計するのがピボットクエリの基本です。