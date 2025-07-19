---
title: "1-3. テーブルの作成 (CREATE TABLE)"
weight: 13
---
テーブルは、リレーショナルデータベースにおけるデータ格納の基本単位です。`CREATE TABLE`文で、どのような列（カラム）を持つテーブルを作成するかを定義します。

{{< highlight sql >}}
CREATE TABLE テーブル名 (
    カラム名1 データ型 制約,
    カラム名2 データ型 制約,
    ...
    カラム名N データ型 制約
);
{{< /highlight >}}

### 主要なデータ型
カラムにどのような種類のデータを格納するかを「データ型」で指定します。以下は代表的なデータ型です。

| 種類 | 代表的なデータ型 | 説明 |
|:---|:---|:---|
| **数値** | `INTEGER` | 整数を格納します。 |
| | `DECIMAL(p, s)` / `NUMERIC(p, s)` | 正確な小数を格納します。`p`は全体の桁数、`s`は小数点以下の桁数。 |
| **文字列** | `VARCHAR(n)` | `n`文字までの可変長の文字列を格納します。 |
| | `CHAR(n)` | `n`文字の固定長の文字列を格納します。 |
| | `TEXT` | 長い文章を格納します。 |
| **日付/時刻** | `DATE` | 日付を格納します (例: '2024-01-01') |
| | `TIMESTAMP` | 日付と時刻を格納します (例: '2024-01-01 12:30:00') |

{{< dbmsdiff title="データ型は方言が多い" >}}
データ型はRDBMSによる違い（方言）が最も大きい部分の一つです。
- **MySQL:** `DATETIME`型がよく使われます。自動採番は `AUTO_INCREMENT`。
- **PostgreSQL:** `SERIAL`型で自動採番を実現します。
- **SQLite:** 実は `INTEGER`, `REAL`, `TEXT`, `BLOB` の4つの型しか持ちません。`VARCHAR(50)` のように書いても、実際は `TEXT` として扱われます。日付も `TEXT` 型で `'YYYY-MM-DD'` のように保存するのが一般的です。自動採番は `INTEGER PRIMARY KEY` に `AUTOINCREMENT` を追加します。
- **MS Access:** `短いテキスト`, `長いテキスト`, `数値型`, `日付/時刻型`, `オートナンバー型` など、GUIで表示される名称が型に対応します。
{{< /dbmsdiff >}}

### テーブルを健全に保つ「制約」
制約は、テーブルに不正なデータが入らないようにするための「ルール」です。

- **`PRIMARY KEY` (主キー制約):**
  - テーブル内で、各行を**一意に識別するため**のカラムに設定します。
  - `NULL`値は許されず、重複も許されません。1つのテーブルに1つだけ設定できます。
- **`NOT NULL` (非NULL制約):**
  - この制約が設定されたカラムには、必ず値を入力しなければなりません (`NULL`は不可)。
- **`UNIQUE` (一意性制約):**
  - テーブル内で値の重複を許しません。`PRIMARY KEY`と似ていますが、`NULL`を許容する（`NULL`同士は重複とみなさない）点が異なります。
- **`FOREIGN KEY` (外部キー制約):**
  - 他のテーブルの主キーを参照するカラムに設定し、テーブル間の**関連性**を定義します。これにより、存在しない部署IDを従業員に設定する、といった不正なデータを防ぎます。
- **`DEFAULT 値` (デフォルト制約):**
  - データ挿入時に値が指定されなかった場合に、自動的に設定される値を指定します。

### `CREATE TABLE` の具体例
演習で使う `employees` テーブルをSQLiteで作成する例です。
{{< highlight sql >}}
CREATE TABLE employees (
    employee_id     INTEGER PRIMARY KEY AUTOINCREMENT,
    employee_name   TEXT NOT NULL,
    hire_date       TEXT,
    salary          INTEGER,
    department_id   INTEGER,
    FOREIGN KEY (department_id) REFERENCES departments(department_id)
);
{{< /highlight >}}
