---
title: "A-5. SQLクイックリファレンス・チートシート"
description: "基本構文から中級テクニックまで、すぐに使えるSQLのチートシート"
weight: 85
---

# SQLクイックリファレンス・チートシート

学習中に迷ったとき、実務での確認用に使えるSQL構文の一覧です。対応しているRDBMSはバッジで示しています。

---


## 🏗 データ定義（DDL）

### データベースの作成・削除
```sql
CREATE DATABASE sample;
DROP DATABASE sample;
```
{{< badges names="MySQL, PostgreSQL, IBM Db2" >}}

*   **補足**: SQLiteおよびMS Accessはファイルベースのデータベースであるため、これらの構文はSQLとしては利用しません。SQLiteはデータベースファイルを作成・削除し、MS AccessはGUI操作やVBAを通じてデータベースファイルを作成・削除します。

---

### テーブルの作成・削除

```sql
CREATE TABLE users (
  id INTEGER PRIMARY KEY,
  name TEXT NOT NULL,
  age INTEGER
);

DROP TABLE users;
```
{{< badges names="標準SQL, MySQL, PostgreSQL, SQLite, IBM Db2, MS Access" >}}

---

### テーブル定義の変更

```sql
ALTER TABLE users ADD COLUMN email TEXT;
```
{{< badges names="標準SQL, MySQL, PostgreSQL, IBM Db2, MS Access" >}}

*   **補足**: SQLite は列の追加のみ対応しており、列の削除や型変更には制限があります。

---

## ✏️ データ操作（DML）

### データの追加

```sql
INSERT INTO users (name, age) VALUES ('Alice', 30);
```
{{< badges names="標準SQL, MySQL, PostgreSQL, SQLite, IBM Db2, MS Access" >}}

---

### データの更新

```sql
UPDATE users SET age = 31 WHERE name = 'Alice';
```
{{< badges names="標準SQL, MySQL, PostgreSQL, SQLite, IBM Db2, MS Access" >}}

---

### データの削除

```sql
DELETE FROM users WHERE name = 'Alice';
```
{{< badges names="標準SQL, MySQL, PostgreSQL, SQLite, IBM Db2, MS Access" >}}

---

## 🔍 データの取得（SELECT）

### 条件指定と並び替え

```sql
SELECT * FROM users WHERE age > 20 ORDER BY age DESC;
```
{{< badges names="標準SQL, MySQL, PostgreSQL, SQLite, IBM Db2, MS Access" >}}

---

### 件数制限

```sql
SELECT * FROM users LIMIT 10 OFFSET 5;
```
{{< badges names="MySQL, PostgreSQL, SQLite" >}}

*   **補足**: `LIMIT`と`OFFSET`は標準SQLではありませんが、多くのRDBMSで独自拡張として採用されています。
    *   **標準SQL / IBM Db2**: `OFFSET 5 ROWS FETCH NEXT 10 ROWS ONLY`のような構文が標準的です。
    *   **MS Access**: `SELECT TOP 10 * FROM users;`のように`TOP`句を使用します。`OFFSET`に直接対応する構文はありません。

---

## 📊 集計・結合

### グループ化・集計

```sql
SELECT age, COUNT(*) FROM users GROUP BY age;
```
{{< badges names="標準SQL, MySQL, PostgreSQL, SQLite, IBM Db2, MS Access" >}}

---

### テーブルの結合

```sql
SELECT * FROM users u INNER JOIN orders o ON u.id = o.user_id;
```
{{< badges names="標準SQL, MySQL, PostgreSQL, SQLite, IBM Db2, MS Access" >}}

---

## 🔣 関数

### 文字列関数

```sql
SELECT UPPER(name), LENGTH(name) FROM users;
```
{{< badges names="標準SQL, MySQL, PostgreSQL, SQLite, IBM Db2" >}}

*   **補足**:
    *   `UPPER`は多くのRDBMSで共通です。
    *   `LENGTH`は文字数またはバイト数を返すなど、RDBMSによって挙動が異なる場合があります（標準SQLでは`CHARACTER_LENGTH`または`CHAR_LENGTH`が文字数を返します）。
    *   **MS Access**: `UCASE(name)`と`LEN(name)`を使用します。

---

### 数値関数

```sql
SELECT ROUND(price, 2), ABS(profit) FROM products;
```
{{< badges names="標準SQL, MySQL, PostgreSQL, SQLite, IBM Db2, MS Access" >}}

---

### 日付関数

```sql
SELECT DATE('now'), STRFTIME('%Y-%m', 'now');
```
{{< badges names="SQLite" >}}

```sql
SELECT CURRENT_DATE, CURRENT_TIME;
```
{{< badges names="標準SQL, MySQL, PostgreSQL, IBM Db2" >}}

*   **補足**:
    *   **MS Access**: `Date()`, `Time()` (関数名の後ろに括弧が必要) や `Now()` などの関数を使用します。`CURRENT_DATE`や`CURRENT_TIME`はサポートしません。

---

## 🧠 中級構文

### サブクエリ

```sql
SELECT name FROM users WHERE id IN (
  SELECT user_id FROM orders WHERE total > 1000
);
```
{{< badges names="標準SQL, MySQL, PostgreSQL, SQLite, IBM Db2, MS Access" >}}

---

### 共通テーブル式（CTE）

```sql
WITH recent_orders AS (
  SELECT * FROM orders WHERE order_date > '2024-01-01'
)
SELECT * FROM recent_orders;
```
{{< badges names="標準SQL, PostgreSQL, SQLite, IBM Db2" >}}

*   **補足**:
    *   **MySQL**: MySQL 8.0以降で対応しています。
    *   **MS Access**: サポートしません。

---

### ウィンドウ関数

```sql
SELECT name, RANK() OVER (ORDER BY score DESC) FROM users;
```
{{< badges names="標準SQL, MySQL, PostgreSQL, SQLite, IBM Db2" >}}

*   **補足**:
    *   **MySQL**: MySQL 8.0以降で対応しています。
    *   **MS Access**: サポートしません。

---

## ⚙ その他便利構文

### 条件分岐（CASE）

```sql
SELECT name,
  CASE
    WHEN age >= 20 THEN '成人'
    ELSE '未成年'
  END AS category
FROM users;
```
{{< badges names="標準SQL, MySQL, PostgreSQL, SQLite, IBM Db2" >}}

*   **補足**:
    *   **MS Access**: `IIF`関数や `Switch` 関数を使用します。`CASE` はサポートしません。

---

### 型変換（CAST）

```sql
SELECT CAST(age AS TEXT) FROM users;
```
{{< badges names="標準SQL, MySQL, PostgreSQL, SQLite, IBM Db2" >}}

*   **補足**:
    *   **MS Access**: `CSTR(age)`や`CINT()`などの変換関数を使用します。`CAST`はサポートしません。

---

### 集合演算

```sql
SELECT name FROM employees
UNION
SELECT name FROM contractors;
```
{{< badges names="標準SQL, MySQL, PostgreSQL, SQLite, IBM Db2, MS Access" >}}

---

### ビュー作成

```sql
CREATE VIEW adult_users AS
SELECT * FROM users WHERE age >= 20;
```
{{< badges names="標準SQL, MySQL, PostgreSQL, SQLite, IBM Db2" >}}

*   **補足**:
    *   **MS Access**: クエリとして保存する機能はありますが、`CREATE VIEW`というSQL構文はサポートしません。

---

### トランザクション制御

```sql
BEGIN;
UPDATE accounts SET balance = balance - 100 WHERE id = 1;
UPDATE accounts SET balance = balance + 100 WHERE id = 2;
COMMIT;
```
{{< badges names="標準SQL, MySQL, PostgreSQL, SQLite, IBM Db2" >}}

*   **補足**:
    *   **MySQL**: `START TRANSACTION;`も利用可能です。
    *   **MS Access**: SQL文としての`BEGIN`や`COMMIT`はサポートしません。VBAのADO/DAOオブジェクトモデルを介してトランザクションを制御します。

---

## 📎 ピボット／アンピボット

### 縦持ち → 横持ち（ピボット）

```sql
SELECT
  user_id,
  MAX(CASE WHEN type = 'A' THEN score END) AS A_score,
  MAX(CASE WHEN type = 'B' THEN score END) AS B_score
FROM scores
GROUP BY user_id;
```
{{< badges names="標準SQL, MySQL, PostgreSQL, SQLite, IBM Db2" >}}

*   **補足**:
    *   この`CASE`文を用いた方法は多くのRDBMSで機能する汎用的な手法です。
    *   **IBM Db2**: `PIVOT`句もサポートしますが、この構文も利用可能です。
    *   **MS Access**: `CASE`文の代わりに`IIF`関数を使うことで同様のロジックを実装できます。

---

### 横持ち → 縦持ち（アンピボット）

```sql
SELECT user_id, 'A' AS type, A_score AS score FROM results
UNION ALL
SELECT user_id, 'B' AS type, B_score FROM results;
```
{{< badges names="標準SQL, MySQL, PostgreSQL, SQLite, IBM Db2, MS Access" >}}

*   **補足**:
    *   この`UNION ALL`を用いた方法は多くのRDBMSで機能する汎用的な手法です。
    *   **IBM Db2**: `UNPIVOT`句もサポートしますが、この構文も利用可能です。

---

## 📝 補足

* このチートシートはSQLの主要構文をカバーしています。
* 各RDBMSのバージョンにより一部構文の使用可否が異なる場合があります。
* 詳細は各章の解説を参照してください。

---

🎉 お疲れ様でした！
本チートシートは常に最新化していく予定です。ご意見・改善提案も歓迎です！

