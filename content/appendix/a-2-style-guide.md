---
title: "A-2. SQLコーディングスタイルガイド"
weight: 82
---

読みやすいSQLは、あなた自身やチームの生産性を高めます。絶対のルールはありませんが、広く使われているスタイルを紹介します。

- **予約語は大文字で書く:** `SELECT`, `FROM`, `WHERE` など。
- **インデント（字下げ）を適切に行う:** `SELECT`句、`FROM`句、`JOIN`句などを明確に区別します。
- **命名規則を統一する:** テーブル名やカラム名は、一貫したルールで命名しましょう。詳細は後述します。
- **`AS`は省略しない:** カラムやテーブルの別名を付ける際は、`AS`を明記すると分かりやすいです。
- **カンマは行の先頭に置くスタイルもある:** `SELECT a, b, c` のように末尾に置くのが一般的ですが、 `SELECT a , b , c` のように先頭に置くスタイルもあります。
- **コメントを活用する:** 複雑なロジックにはコメントを残しましょう。`--` で一行コメント、`/* ... */`で複数行コメント。

---

### 命名規則に関する推奨事項

**原則として、テーブル名やカラム名には英数字とアンダースコア(`_`)のみを使いましょう。**
空白、日本語、ハイフンなどの特殊文字、SQLの予約語（`ORDER`, `GROUP`など）の使用は、予期せぬエラーの原因となるため避けるのが賢明です。

- **悪い例:** `顧客 テーブル`, `商品-マスタ`, `order` (予約語)
- **良い例:** `customer_tables`, `product_master`, `orders`

#### やむを得ず特殊文字などを使う場合
どうしても空白や特殊文字を含む名前を扱う必要がある場合、各RDBMSが定める「識別子のクォート文字」で囲む必要があります。これはDBMSごとに異なります。

- **MySQL:** バッククォート (`` ` ``)
  ```sql
  SELECT * FROM `顧客 テーブル`;
  ```
- **PostgreSQL, 標準SQL:** ダブルクォート (`"`)
  ```sql
  SELECT * FROM "商品-マスタ";
  ```
- **SQL Server, MS Access:** 角括弧 (`[]`)
  ```sql
  SELECT * FROM [order];
  ```
- **SQLite:** `""`, `` ` ``, `[]` のいずれも受け入れることが多いですが、互換性の観点からは標準SQLに準拠したダブルクォート(`"`)の使用が推奨されます。

このように、毎回クォート文字で囲むのは手間がかかり、クエリが読みにくくなります。また、データベースの種類を変更する際の障害にもなり得ます。特別な理由がない限り、シンプルでクリーンな命名規則に従うことを強くお勧めします。

---

**良い例 (推奨スタイル適用後):**
{{< highlight sql >}}
/*
  2023年にPCまたは周辺機器を購入した
  関東地方の顧客ごとの合計売上金額を計算する
*/
WITH sales_2023 AS (
    -- 2023年の売上のみを抽出
    SELECT
        sale_id,
        customer_id,
        product_id,
        quantity
    FROM
        sales
    WHERE
        STRFTIME('%Y', sale_date) = '2023'
)
SELECT
    c.customer_name,
    SUM(p.price * s.quantity) AS total_amount
FROM
    sales_2023 AS s
    INNER JOIN customers AS c
        ON s.customer_id = c.customer_id
    INNER JOIN products AS p
        ON s.product_id = p.product_id
WHERE
    c.region = '関東'
    AND p.category IN ('PC', '周辺機器')
GROUP BY
    c.customer_name
ORDER BY
    total_amount DESC;
{{< /highlight >}}