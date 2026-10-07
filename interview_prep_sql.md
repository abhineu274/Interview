# SQL Interview Prep — Top Query Questions

> Classic SQL query questions asked in almost every technical interview. Each includes the problem, sample table, solution query (SQL Server syntax, with notes for PostgreSQL/MySQL differences), and explanation.

---

### 1. Nth Highest Salary

**Problem:** Find the Nth highest salary from an `Employees` table.

```sql
-- Using DENSE_RANK (handles ties correctly, recommended)
SELECT Salary
FROM (
    SELECT Salary, DENSE_RANK() OVER (ORDER BY Salary DESC) AS rnk
    FROM Employees
) t
WHERE rnk = N; -- replace N with the desired rank, e.g. 3


SELECT Name, Salary --Name, Salary
FROM (
    SELECT Name, Salary, DENSE_RANK() OVER (ORDER BY Salary DESC) AS rnk
    FROM Employees
) t
WHERE rnk = N; -- replace N with the desired rank, e.g. 3
```

```sql
-- Classic subquery approach (no window functions)
SELECT MIN(Salary)
FROM (
    SELECT DISTINCT TOP (N) Salary   -- MySQL/Postgres: LIMIT N
    FROM Employees
    ORDER BY Salary DESC
) t;
```

**Key Point:** `DENSE_RANK` is preferred because it correctly handles duplicate salaries (two people tied for 2nd place both get rank 2, next is rank 3, not 4).

---

### 2. Second Highest Salary (special case of Nth)

```sql
SELECT MAX(Salary) AS SecondHighest
FROM Employees
WHERE Salary < (SELECT MAX(Salary) FROM Employees);
```

**Key Point:** This avoids `OFFSET`/`LIMIT` entirely and works everywhere — a common quick-answer version interviewers like to see alongside the general Nth-highest solution.

---

### 3. Remove Duplicate Rows (keep one copy)

**Problem:** Table has duplicate rows (same values across all/some columns); delete duplicates but keep one.

```sql
-- Using a CTE with ROW_NUMBER (SQL Server/Postgres)
WITH CTE AS (
    SELECT *, ROW_NUMBER() OVER (
        PARTITION BY Name, Email  -- columns that define a "duplicate"
        ORDER BY Id
    ) AS rn
    FROM Employees
)
DELETE FROM CTE WHERE rn > 1;
```

**Key Point:** `PARTITION BY` groups rows considered duplicates; `ORDER BY Id` decides which copy survives (rn = 1, usually the oldest/first inserted).

---

### 4. Find Duplicate Rows (without deleting)

```sql
SELECT Name, Email, COUNT(*) AS DuplicateCount
FROM Employees
GROUP BY Name, Email
HAVING COUNT(*) > 1;
```

**Key Point:** `HAVING` filters on the aggregated result (`COUNT`), whereas `WHERE` cannot reference aggregate functions — a common "why not WHERE" follow-up.

---

### 5. Maximum Salary (overall and per department)

```sql
-- Overall max
SELECT MAX(Salary) AS MaxSalary FROM Employees;

-- Max salary per department
SELECT DepartmentId, MAX(Salary) AS MaxSalary
FROM Employees
GROUP BY DepartmentId;
```

---

### 6. Employees Earning More Than Their Manager (Self Join)

```sql
SELECT e.Name AS Employee, e.Salary, m.Name AS Manager, m.Salary AS ManagerSalary
FROM Employees e
JOIN Employees m ON e.ManagerId = m.Id
WHERE e.Salary > m.Salary;
```

**Key Point:** Classic self-join — the same table is joined to itself using an alias twice, once as "employee" and once as "manager."

---

### 7. Highest Salary in Each Department (Top-1-per-group)

```sql
SELECT DepartmentId, Name, Salary
FROM (
    SELECT DepartmentId, Name, Salary,
           RANK() OVER (PARTITION BY DepartmentId ORDER BY Salary DESC) AS rnk
    FROM Employees
) t
WHERE rnk = 1;
```

**Key Point:** This is the "Top-N per group" pattern — one of the most frequently asked window-function questions. Swap `RANK` for `ROW_NUMBER` if ties should only return exactly one row per department.

---

### 8. Second Highest Salary Per Department

```sql
SELECT DepartmentId, Name, Salary
FROM (
    SELECT DepartmentId, Name, Salary,
           DENSE_RANK() OVER (PARTITION BY DepartmentId ORDER BY Salary DESC) AS rnk
    FROM Employees
) t
WHERE rnk = 2;
```

---

### 9. Count of Employees in Each Department (including departments with zero employees)

```sql
SELECT d.DepartmentName, COUNT(e.Id) AS EmployeeCount
FROM Departments d
LEFT JOIN Employees e ON d.Id = e.DepartmentId
GROUP BY d.DepartmentName;
```

**Key Point:** `LEFT JOIN` (not `INNER JOIN`) is essential here so departments with no employees still show up with a count of 0 — a very common interviewer trap.

---

### 10. Find Employees With No Department Assigned

```sql
SELECT e.Name
FROM Employees e
LEFT JOIN Departments d ON e.DepartmentId = d.Id
WHERE d.Id IS NULL;
```

**Key Point:** `LEFT JOIN` + `WHERE ... IS NULL` is the standard pattern for "find records in A that have no match in B."

---

### 11. Running Total / Cumulative Sum

```sql
SELECT EmployeeId, OrderDate, Amount,
       SUM(Amount) OVER (ORDER BY OrderDate 
                          ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW) AS RunningTotal
FROM Orders;
```

**Key Point:** Window functions (`OVER`) let you compute aggregates like running totals without collapsing rows via `GROUP BY` — you keep every row and add a calculated column.

---

### 12. Rank vs Dense_Rank vs Row_Number (conceptual + query)

```sql
SELECT Name, Salary,
       ROW_NUMBER() OVER (ORDER BY Salary DESC) AS RowNum,
       RANK()       OVER (ORDER BY Salary DESC) AS Rnk,
       DENSE_RANK() OVER (ORDER BY Salary DESC) AS DenseRnk
FROM Employees;
```

**Key Point (common verbal follow-up):**
- `ROW_NUMBER()` — always unique, sequential (1,2,3,4...), even for ties.
- `RANK()` — ties share the same rank, but leaves gaps afterward (1,2,2,4).
- `DENSE_RANK()` — ties share the same rank, no gaps afterward (1,2,2,3).

---

### 13. Employees Who Joined in the Last N Days / Current Month

```sql
-- Last 30 days
SELECT * FROM Employees
WHERE JoinDate >= DATEADD(DAY, -30, GETDATE());  -- Postgres: JoinDate >= CURRENT_DATE - INTERVAL '30 days'

-- Current month
SELECT * FROM Employees
WHERE MONTH(JoinDate) = MONTH(GETDATE()) AND YEAR(JoinDate) = YEAR(GETDATE());
```

---

### 14. UNION vs UNION ALL

```sql
SELECT Name FROM Employees WHERE DepartmentId = 1
UNION
SELECT Name FROM Employees WHERE DepartmentId = 2;
```

**Key Point:** `UNION` removes duplicate rows across the combined result (does an implicit `DISTINCT`, so it's slower); `UNION ALL` keeps all rows including duplicates and is faster — use `UNION ALL` whenever you know there won't be (or don't care about) duplicates.

---

### 15. Second Highest Value Without Any Window Functions or Subquery Tricks (OFFSET/FETCH)

```sql
-- SQL Server
SELECT DISTINCT Salary
FROM Employees
ORDER BY Salary DESC
OFFSET 1 ROWS FETCH NEXT 1 ROWS ONLY;

-- PostgreSQL / MySQL
SELECT DISTINCT Salary
FROM Employees
ORDER BY Salary DESC
LIMIT 1 OFFSET 1;
```

**Key Point:** `OFFSET`/`FETCH` (or `LIMIT`/`OFFSET`) is the simplest pagination-style approach — good to mention as an alternative to the `DENSE_RANK` solution in Q1, showing you know multiple ways to solve the same problem.

---

## Quick-Reference Summary Table

| # | Question | Core Technique |
|---|---|---|
| 1 | Nth highest salary | `DENSE_RANK()` / subquery with `TOP`+`MIN` |
| 2 | Second highest salary | `MAX(Salary) WHERE Salary < MAX(Salary)` |
| 3 | Remove duplicate rows | `ROW_NUMBER()` + `DELETE WHERE rn > 1` |
| 4 | Find duplicate rows | `GROUP BY` + `HAVING COUNT(*) > 1` |
| 5 | Max salary (overall/per group) | `MAX()` + `GROUP BY` |
| 6 | Employees earning more than manager | Self `JOIN` |
| 7 | Top salary per department | `RANK()`/`ROW_NUMBER()` + `PARTITION BY` |
| 8 | 2nd highest per department | `DENSE_RANK()` + `PARTITION BY` |
| 9 | Employee count per department (incl. 0) | `LEFT JOIN` + `GROUP BY` |
| 10 | Employees with no department | `LEFT JOIN` + `WHERE ... IS NULL` |
| 11 | Running total | Window `SUM() OVER (ORDER BY ...)` |
| 12 | Rank vs Dense_Rank vs Row_Number | Conceptual + `OVER()` |
| 13 | Date-range filtering | `DATEADD`/`INTERVAL`, `MONTH()`/`YEAR()` |
| 14 | UNION vs UNION ALL | Set operators, dedup behavior |
| 15 | Pagination-style Nth row | `OFFSET`/`FETCH` or `LIMIT`/`OFFSET` |

## Interview Tips
- Always clarify: "Should ties be handled?" before choosing `RANK()` vs `DENSE_RANK()` vs `ROW_NUMBER()`.
- If asked for the Nth highest generically, give the window-function solution first (scales to any N cleanly), then mention the subquery/`OFFSET` alternatives if asked "any other way?"
- Know **why** `HAVING` is needed instead of `WHERE` for aggregate filtering — a very common one-line follow-up.
- Be ready to explain time complexity/performance trade-offs: indexing on `ORDER BY`/`PARTITION BY` columns matters for all of these at scale.

---

## SQL Concepts — One-Liners

- **Primary Key vs Unique Key** — Primary key disallows `NULL` and there can only be one per table; unique key allows a single `NULL` (in most DBs) and a table can have several.
- **Clustered vs Non-Clustered Index** — Clustered index physically sorts/stores table data in index order (one per table); non-clustered index is a separate structure with pointers back to the data (many allowed).
- **WHERE vs HAVING** — `WHERE` filters rows before grouping/aggregation; `HAVING` filters groups after `GROUP BY`/aggregation.
- **DELETE vs TRUNCATE vs DROP** — `DELETE` removes rows (logged, can be rolled back, fires triggers); `TRUNCATE` removes all rows fast (minimal logging, resets identity); `DROP` removes the whole table structure.
- **Normalization** — Organizing tables/columns to reduce data redundancy and avoid update/insert/delete anomalies (1NF → atomic values, 2NF → no partial dependency, 3NF → no transitive dependency).
- **Denormalization** — Intentionally adding redundancy (e.g., duplicating columns) to trade storage for faster reads, common in reporting/data-warehouse tables.
- **ACID** — Atomicity, Consistency, Isolation, Durability — the guarantees a transaction provides.
- **Isolation Levels** — `READ UNCOMMITTED` < `READ COMMITTED` < `REPEATABLE READ` < `SERIALIZABLE`, each trading concurrency for protection against dirty/non-repeatable/phantom reads.
- **INNER vs LEFT vs RIGHT vs FULL JOIN** — `INNER` returns matching rows only; `LEFT`/`RIGHT` keep all rows from one side plus matches; `FULL` keeps all rows from both sides.
- **Correlated Subquery** — A subquery that references a column from the outer query, so it re-executes once per outer row (vs a plain subquery that runs once).
- **CTE vs Subquery** — A CTE (`WITH ... AS (...)`) is a named, more readable temporary result set (can be recursive); a subquery is inline and can't reference itself.
- **View vs Materialized View** — A view is a stored query with no data of its own (always live); a materialized view stores the query's result physically and must be refreshed.
- **Index Trade-off** — Indexes speed up reads (`SELECT`/`WHERE`/`JOIN`) but slow down writes (`INSERT`/`UPDATE`/`DELETE`) because the index must also be maintained.
- **Composite Index Column Order** — Put the most selective / most frequently filtered-on column first, since a composite index can only be used efficiently as a left-to-right prefix.
- **NULL Handling** — `NULL` means "unknown," so `NULL = NULL` evaluates to `UNKNOWN`, not `TRUE`; always use `IS NULL` / `IS NOT NULL` to check for it.
- **Foreign Key** — Enforces referential integrity by requiring a column's value to match a primary/unique key in another (or the same) table.
- **Stored Procedure vs Function** — Procedures can perform DML and don't have to return a value (called via `EXEC`); functions must return a value and can be used inline in a `SELECT` (but typically can't perform DML).
- **Transaction Locking Problems** — Dirty read (reads uncommitted data), non-repeatable read (same row changes between reads), phantom read (new rows appear between reads within a transaction).
- **EXISTS vs IN** — `EXISTS` stops at the first match and handles `NULL`s safely, generally preferred for large/correlated subqueries; `IN` materializes the full list and can misbehave with `NULL` values in the subquery result.
- **Index on Foreign Keys** — Not created automatically by most databases; explicitly add one to speed up joins and avoid table locks on the parent during child deletes/updates.
- **Query Execution Order (logical)** — `FROM` → `WHERE` → `GROUP BY` → `HAVING` → `SELECT` → `ORDER BY` → `LIMIT`/`OFFSET` (this is why aliases from `SELECT` can't be used in `WHERE`).
- **GROUP BY Rule** — Every non-aggregated column in `SELECT` must appear in `GROUP BY` (SQL Server/Postgres enforce this; MySQL allows it but the result is undefined without `ONLY_FULL_GROUP_BY` off).
- **GROUP BY with Multiple Columns** — Groups rows by the unique combination of all listed columns, so each output row represents one distinct combination (e.g., `GROUP BY DepartmentId, JobTitle`).
- **HAVING without GROUP BY** — Legal in most DBs; treats the whole table as one group, so `HAVING COUNT(*) > 5` filters based on a single aggregate over all rows.
- **Multiple HAVING Conditions** — Combine with `AND`/`OR` just like `WHERE`, e.g. `HAVING COUNT(*) > 5 AND AVG(Salary) > 50000`.
- **DISTINCT vs GROUP BY** — Both can deduplicate rows, but `GROUP BY` is needed when you also want to compute per-group aggregates; `DISTINCT` is just for unique rows/columns with no aggregation.
- **ROLLUP / CUBE / GROUPING SETS** — `ROLLUP` adds subtotal + grand-total rows along a hierarchy; `CUBE` adds subtotals for every combination of grouped columns; `GROUPING SETS` lets you specify exactly which grouping combinations to compute in one query.
- **Aggregate Functions and NULL** — `COUNT(*)` counts all rows including `NULL`s; `COUNT(column)`, `SUM`, `AVG`, `MIN`, `MAX` all ignore `NULL` values in that column.
- **CASE Expression in Aggregates** — Conditional aggregation pattern, e.g. `SUM(CASE WHEN Gender = 'F' THEN 1 ELSE 0 END)`, used to pivot data without a dedicated `PIVOT` operator.
- **Filtering Before vs After Aggregation** — Put row-level conditions in `WHERE` (cheaper, filters early) and only use `HAVING` for conditions on the aggregated result — a common performance follow-up.
- **COALESCE vs ISNULL** — `COALESCE(a, b, c, ...)` is ANSI-standard and takes multiple arguments, returning the first non-`NULL`; `ISNULL(a, b)` is SQL-Server-specific and takes exactly two.
- **Scalar vs Aggregate vs Window Function** — Scalar functions return one value per row (`UPPER()`), aggregate functions collapse many rows into one (`SUM()`), window functions compute per-row values across a set of related rows without collapsing them (`SUM() OVER (...)`).

---

## Stored Procedures & Table-Valued Parameters (SQL Server)

**Basic stored procedure:**

```sql
CREATE PROCEDURE GetEmployeesByDepartment @DeptId INT
AS
    SELECT Name, Salary FROM Employees WHERE DepartmentId = @DeptId;
GO
```

```sql
EXEC GetEmployeesByDepartment @DeptId = 3;
```

**Stored procedure with output parameter:**

```sql
CREATE PROCEDURE GetEmployeeCount @DeptId INT, @Total INT OUTPUT
AS
    SELECT @Total = COUNT(*) FROM Employees WHERE DepartmentId = @DeptId;
GO
```

**Table-Valued Parameter (TVP) — pass a whole table into a proc:**

```sql
-- 1. Define a table type
CREATE TYPE EmployeeIdList AS TABLE (Id INT);

-- 2. Proc that accepts the TVP (must be passed READONLY)
CREATE PROCEDURE GetEmployeesByIds @Ids EmployeeIdList READONLY
AS
    SELECT e.Name, e.Salary
    FROM Employees e
    JOIN @Ids i ON e.Id = i.Id;
GO
```

```sql
-- 3. Declare, populate, and pass the TVP
DECLARE @IdList EmployeeIdList;
INSERT INTO @IdList VALUES (1), (2), (5);

EXEC GetEmployeesByIds @Ids = @IdList;
```

**Key Point:** TVPs let you send a set of rows to SQL Server in a single round trip (instead of a comma-separated string or multiple calls) — commonly asked as "how do you pass a list/array to a stored procedure?" A TVP parameter must be declared `READONLY` inside the proc.
