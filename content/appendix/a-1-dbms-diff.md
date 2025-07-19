---
title: "A-1. 対象DBMSの特徴と差異まとめ表"
weight: 81
---

# SQL 機能対応表：RDBMS別互換性リファレンス

このドキュメントは、主要なSQL機能が「標準SQL」および「MySQL、PostgreSQL、SQLite、IBM Db2、MS Access」の各RDBMSでどのようにサポートされているかをまとめたものです。データベースの設計やクエリ作成の際の参考としてご活用ください。

## SQL 機能対応表

| 機能/項目 | 標準SQL | MySQL | PostgreSQL | SQLite | IBM Db2 | MS Access |
|:---|:---|:---|:---|:---|:---|:---|
| **自動採番** | `IDENTITY` | `AUTO_INCREMENT` / `GENERATED AS IDENTITY` (8.0+) | `SERIAL` / `GENERATED AS IDENTITY` | `INTEGER PRIMARY KEY` / `AUTOINCREMENT` | `IDENTITY` | `オートナンバー` |
| **日付型** | `DATE` | `DATE` | `DATE` | `TEXT`推奨 / `REAL` / `INTEGER` | `DATE` | `日付/時刻型` |
| **件数制限** | `OFFSET ... FETCH FIRST` | `LIMIT` / `OFFSET ... FETCH` (8.0+) | `LIMIT` / `OFFSET ... FETCH` | `LIMIT` | `OFFSET ... FETCH FIRST` | `TOP` |
| **文字列結合** | `\|\|` | `CONCAT()` / `\|\|` (PIPES_AS_CONCAT) | `\|\|` / `CONCAT()` | `\|\|` | `\|\|` / `CONCAT()` | `&` / `+` |
| **ウィンドウ関数** | ✔ | ✔ (8.0+) | ✔ | ✔ (3.25.0+) | ✔ | ✖ |
| **CTE (`WITH`)** | ✔ | ✔ (8.0+) | ✔ | ✔ (3.8.3+) | ✔ | ✖ |
| **`RIGHT JOIN`** | ✔ | ✔ | ✔ | ✖ | ✔ | ✔ |
| **`FULL OUTER JOIN`** | ✔ | ✖ | ✔ | ✖ | ✔ | ✖ |
| **`INTERSECT`** | ✔ | ✔ (8.0.31+, DISTINCTのみ) | ✔ | ✔ | ✔ | ✖ |
| **`EXCEPT`** | ✔ | ✔ (8.0.31+, DISTINCTのみ) | ✔ | ✔ | ✔ | ✖ |
| **トランザクション** | `START TRANSACTION` | `START TRANSACTION` / `BEGIN` | `BEGIN` / `START TRANSACTION` | `BEGIN` / `BEGIN TRANSACTION` | `START TRANSACTION` / `BEGIN WORK` | △ (VBA経由) |

## 各機能の補足説明

### 自動採番
- **PostgreSQL**: `SERIAL`はPostgreSQL独自の擬似データ型で、内部的にはシーケンスとデフォルト値を組み合わせたもの。標準SQL準拠の`GENERATED AS IDENTITY`（PostgreSQL 10以降）も利用可能。
- **MySQL**: `AUTO_INCREMENT`が一般的。MySQL 8.0以降では標準SQL準拠の`GENERATED AS IDENTITY`もサポート（例: `id INT GENERATED ALWAYS AS IDENTITY`）。
- **SQLite**: `AUTOINCREMENT`は`INTEGER PRIMARY KEY`に追加の保証（ROWIDの再利用防止）を提供。通常は`INTEGER PRIMARY KEY`で自動採番可能。

### 日付型
- **SQLite**: 専用`DATE`型はない。`TEXT`（ISO8601形式：`YYYY-MM-DD`）が推奨されるが、`REAL`（Julian日）や`INTEGER`（Unixタイムスタンプ）も使用可能。例: `TEXT`で`'2025-07-19'`、または`INTEGER`で`1626393600`。

### 件数制限
- **標準SQL**: `FETCH FIRST`は単独でも使用可能だが、`OFFSET n ROWS FETCH NEXT m ROWS ONLY`が完全な構文。
- **MySQL**: `LIMIT n`が一般的。MySQL 8.0以降で標準SQL準拠の`OFFSET n ROWS FETCH NEXT m ROWS ONLY`もサポート。
- **IBM Db2**: `FETCH FIRST n ROWS ONLY`や`OFFSET n ROWS FETCH NEXT m ROWS ONLY`が一般的。
- **MS Access**: `SELECT TOP N ...`を使用。`OFFSET`は直接サポートされない。

### 文字列結合
- **MySQL**: `||`はデフォルトで論理OR。文字列結合には`SET SQL_MODE='PIPES_AS_CONCAT'`が必要だが、`CONCAT()`が安全で推奨。
- **MS Access**: `+`はNULL値を含む場合に結果がNULLになる可能性があるため、`&`が推奨。

### `INTERSECT` と `EXCEPT`
- **MySQL**: MySQL 8.0.31以降で`INTERSECT DISTINCT`および`EXCEPT DISTINCT`をサポート。`ALL`オプションは非サポート。
- **MS Access**: 直接サポートなし。`INTERSECT`は`INNER JOIN`、 `EXCEPT`は`LEFT JOIN`や`NOT IN`でエミュレート可能。

### トランザクション
- **MySQL**: `BEGIN`は`START TRANSACTION`のエイリアス。
- **PostgreSQL**: `BEGIN`が一般的だが、`START TRANSACTION`も可。
- **SQLite**: `BEGIN`または`BEGIN TRANSACTION`を使用。
- **IBM Db2**: `START TRANSACTION`、`BEGIN WORK`、`BEGIN TRANSACTION`が使用可能。
- **MS Access**: SQLで直接`BEGIN`や`COMMIT`は不可。VBAのDAO/ADOで`BeginTrans`、 `CommitTrans`、 `Rollback`を使用（例: `CurrentDb.Execute "BeginTrans"`）。

---

### **変更点の概要**
1. **自動採番**: MySQLの`GENERATED AS IDENTITY`（8.0+）を表と補足に追加。
2. **日付型**: SQLiteで`REAL`と`INTEGER`の使用可能性を補足に追加。
3. **件数制限**: MySQLの`OFFSET ... FETCH`（8.0+）を表と補足に追加。
4. **INTERSECT/EXCEPT**: MySQLの`DISTINCT`必須と`ALL`非サポートを明確化。
5. **トランザクション**: MS AccessのVBA具体例（`BeginTrans`等）を補足に追加。

この修正版は最新情報（2025年7月19日時点）を反映し、より包括的かつ正確です。さらに詳細な説明や特定のRDBMSのコード例が必要な場合は、ご指示ください！