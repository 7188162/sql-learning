---
title: "2-1. 【Read】データの検索と条件指定"
weight: 21
---
`SELECT`文は、テーブルからデータを取得するための、SQLで最も重要かつ頻繁に使われるコマンドです。

### 全ての列、特定の列を取得する
`employees` テーブルを例に見てみましょう。

- **全ての列を取得 (`*`)**
  アスタリスク(`*`)は「すべての列」を意味するワイルドカードです。
  {{< highlight sql >}}
  SELECT * FROM employees;
  {{< /highlight >}}

- **特定の列だけを取得**
  必要な列の名前をカンマ区切りで指定します。
  {{< highlight sql >}}
  SELECT employee_name, salary FROM employees;
  {{< /highlight >}}

### 条件を指定して絞り込む (`WHERE`)
`WHERE`句を使うと、特定の条件に一致する行だけを抽出できます。

- **比較演算子 (`=`, `>`, `<`など)**
  営業部（`department_id`が10）の従業員だけを取得します。
  {{< highlight sql >}}
  SELECT * FROM employees WHERE department_id = 10;
  {{< /highlight >}}
  
  給与(`salary`)が300,000より大きい従業員を取得します。
  {{< highlight sql >}}
  SELECT employee_name, salary FROM employees WHERE salary > 300000;
  {{< /highlight >}}

- **論理演算子 (`AND`, `OR`, `NOT`)**
  複数の条件を組み合わせることができます。
  {{< highlight sql >}}
  -- 開発部(20) 'かつ' 給与が350,000以上の従業員
  SELECT * FROM employees WHERE department_id = 20 AND salary >= 350000;
  
  -- 営業部(10) 'または' 人事部(30)の従業員
  SELECT * FROM employees WHERE department_id = 10 OR department_id = 30;
  {{< /highlight >}}

- **範囲・リスト (`BETWEEN`, `IN`)**
  `BETWEEN`は範囲指定、`IN`はリスト内のいずれかに一致するかを調べます。
  {{< highlight sql >}}
  -- 給与が300,000から350,000の間の従業員
  SELECT * FROM employees WHERE salary BETWEEN 300000 AND 350000;

  -- 営業部(10)または人事部(30)の従業員 (ORの書き換え)
  SELECT * FROM employees WHERE department_id IN (10, 30);
  {{< /highlight >}}

- **パターンマッチ (`LIKE`)**
  `LIKE`は文字列の一部が一致するものを探します。`%`は「0文字以上の任意の文字列」、`_`は「任意の1文字」を意味します。
  {{< highlight sql >}}
  -- 名前に「郎」を含む従業員
  SELECT * FROM employees WHERE employee_name LIKE '%郎%';
  {{< /highlight >}}

- **NULLの判定 (`IS NULL`)**
  `NULL`（値が存在しない状態）のデータを検索するには `=` ではなく `IS NULL` を使います。
  {{< highlight sql >}}
  -- 部署に所属していない従業員
  SELECT * FROM employees WHERE department_id IS NULL;
  {{< /highlight >}}
