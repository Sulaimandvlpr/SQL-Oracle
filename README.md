# Oracle SQL — Database Engineering Notes

> **A personal technical handbook for learning, understanding, and practicing Oracle SQL.**

![Oracle SQL](https://img.shields.io/badge/Oracle%20SQL-Database-red)
![Oracle Database 26ai](https://img.shields.io/badge/Oracle%20Database-26ai-orange)
![Status](https://img.shields.io/badge/Status-Active%20Learning-blue)
![Focus](https://img.shields.io/badge/Focus-SQL%20%7C%20Database%20%7C%20PL%2FSQL-green)
![Author](https://img.shields.io/badge/Author-Sulaimandvlpr-black)

---

## About This Repository

**Oracle SQL — Database Engineering Notes** adalah catatan belajar pribadi yang dibuat untuk mendokumentasikan proses mempelajari **database, relational database, Oracle SQL, dan PL/SQL** secara terstruktur.

Repository ini bukan sekadar kumpulan syntax SQL.

Setiap materi dirancang agar tidak hanya menjawab:

> "Apa syntax-nya?"

tetapi juga:

> "Apa konsepnya?"
> "Mengapa digunakan?"
> "Kapan digunakan?"
> "Bagaimana cara kerjanya?"
> "Bagaimana penerapannya dalam sistem nyata?"

---

## Author

**Sulaimandvlpr**

Computer Science / Informatics Student
Focused on Software Engineering, Data, and Database Technology.

Repository ini dibuat sebagai **living documentation** dari proses belajar dan pengembangan kemampuan database.

> Learn. Build. Document. Improve.

---

# Why This Repository Exists

SQL sering dipelajari hanya melalui hafalan syntax:

```sql
SELECT * FROM table;
```

Padahal kemampuan database yang baik membutuhkan pemahaman yang jauh lebih luas.

Seorang developer atau data professional perlu memahami bagaimana data:

* dimodelkan
* disimpan
* dihubungkan
* di-query
* dimanipulasi
* divalidasi
* diamankan
* ditransaksikan
* dioptimalkan

Karena itu, repository ini dibuat dengan pendekatan:

```text
CONCEPT
   ↓
DATABASE DESIGN
   ↓
SQL
   ↓
ORACLE
   ↓
PRACTICE
   ↓
PROBLEM SOLVING
   ↓
PERFORMANCE
   ↓
SECURITY
   ↓
REAL-WORLD APPLICATION
```

---

# Learning Philosophy

Repository ini menggunakan beberapa prinsip.

### 01 — Understand Before Memorizing

Syntax bukan tujuan utama.

Pemahaman konsep menjadi prioritas sebelum menghafal query.

### 02 — Practice With Real Data

Setiap konsep sebisa mungkin diterapkan menggunakan dataset dan kasus yang menyerupai kondisi nyata.

### 03 — Think Like a Database Engineer

Tidak hanya bertanya:

> "Apakah query ini berjalan?"

Tetapi juga:

> "Apakah query ini benar?"
> "Apakah desainnya baik?"
> "Apakah datanya konsisten?"
> "Apakah query-nya efisien?"
> "Apakah aman?"

### 04 — Documentation First

Setiap materi didokumentasikan agar dapat digunakan kembali sebagai reference.

### 05 — Industry-Aware

Materi tidak hanya berorientasi pada kebutuhan akademik, tetapi juga memperhatikan praktik yang relevan dengan pekerjaan database dan software engineering.

---

# Scope

Repository ini mencakup beberapa area utama.

```text
Database Fundamentals
        │
        ├── DBMS
        ├── RDBMS
        ├── Relational Model
        ├── Tables
        ├── Keys
        ├── Constraints
        └── Relationships

Database Design
        │
        ├── ERD
        ├── Normalization
        ├── 1NF
        ├── 2NF
        ├── 3NF
        └── BCNF

Oracle SQL
        │
        ├── DDL
        ├── DML
        ├── DQL
        ├── TCL
        ├── DCL
        ├── Functions
        ├── JOIN
        ├── Subquery
        ├── CTE
        ├── Set Operators
        └── Analytic Functions

Oracle Database
        │
        ├── Tables
        ├── Views
        ├── Indexes
        ├── Sequences
        ├── Synonyms
        └── Database Objects

PL/SQL
        │
        ├── Variables
        ├── Conditions
        ├── Loops
        ├── Cursors
        ├── Procedures
        ├── Functions
        ├── Triggers
        └── Packages

Professional Database Skills
        │
        ├── Transactions
        ├── Concurrency
        ├── Query Optimization
        ├── Execution Plans
        ├── Indexing
        ├── Security
        └── SQL Injection Prevention
```

---

# Roadmap

## Phase 01 — Database Fundamentals

* Data
* Database
* DBMS
* RDBMS
* Relational Database
* Table
* Row
* Column
* Schema
* Entity
* Attribute
* Relationship
* Primary Key
* Foreign Key
* Candidate Key
* Composite Key
* Constraints
* Data Integrity
* Referential Integrity

---

## Phase 02 — Database Design

* ERD
* Cardinality
* Relationship Types
* Functional Dependency
* Normalization
* 1NF
* 2NF
* 3NF
* BCNF
* Data Redundancy
* Insert Anomaly
* Update Anomaly
* Delete Anomaly

---

## Phase 03 — Oracle Fundamentals

* Oracle Database
* Oracle Architecture
* User
* Schema
* Tablespace
* Database Object
* SQL Developer
* SQLcl
* Oracle Data Types
* `NUMBER`
* `VARCHAR2`
* `DATE`
* `TIMESTAMP`
* `CLOB`
* `BLOB`

---

## Phase 04 — SQL Fundamentals

### DDL

```text
CREATE
ALTER
DROP
TRUNCATE
RENAME
COMMENT
```

### DML

```text
INSERT
UPDATE
DELETE
MERGE
```

### Query

```text
SELECT
FROM
WHERE
DISTINCT
ORDER BY
GROUP BY
HAVING
FETCH
OFFSET
```

### TCL

```text
COMMIT
ROLLBACK
SAVEPOINT
```

### DCL

```text
GRANT
REVOKE
```

---

## Phase 05 — SQL Functions

### Character Functions

```text
UPPER()
LOWER()
INITCAP()
LENGTH()
SUBSTR()
INSTR()
REPLACE()
TRIM()
```

### Number Functions

```text
ROUND()
TRUNC()
CEIL()
FLOOR()
MOD()
ABS()
```

### Date Functions

```text
SYSDATE
SYSTIMESTAMP
ADD_MONTHS()
MONTHS_BETWEEN()
NEXT_DAY()
LAST_DAY()
```

### Conversion Functions

```text
TO_CHAR()
TO_DATE()
TO_NUMBER()
```

### NULL Functions

```text
NVL()
NVL2()
NULLIF()
COALESCE()
```

### Aggregate Functions

```text
COUNT()
SUM()
AVG()
MIN()
MAX()
```

---

## Phase 06 — Querying Multiple Tables

* INNER JOIN
* LEFT JOIN
* RIGHT JOIN
* FULL OUTER JOIN
* CROSS JOIN
* SELF JOIN
* Multiple JOIN
* Join Conditions
* Non-Equi Join

---

## Phase 07 — Advanced Query

* Subquery
* Single-Row Subquery
* Multiple-Row Subquery
* Correlated Subquery
* `IN`
* `EXISTS`
* `ANY`
* `ALL`
* Common Table Expression
* `WITH`
* Set Operators
* `UNION`
* `UNION ALL`
* `INTERSECT`
* `MINUS`
* `CASE`

---

## Phase 08 — Analytic & Window Functions

* `OVER()`
* `PARTITION BY`
* `ORDER BY`
* `ROW_NUMBER()`
* `RANK()`
* `DENSE_RANK()`
* `LAG()`
* `LEAD()`
* Running Total
* Moving Average
* Ranking
* Analytical Aggregation

---

## Phase 09 — Oracle Database Objects

* View
* Index
* Sequence
* Synonym
* Materialized View
* Database Link
* Temporary Table

---

## Phase 10 — Transaction & Concurrency

* Transaction
* ACID
* COMMIT
* ROLLBACK
* SAVEPOINT
* Lock
* Row-Level Lock
* Concurrency
* Isolation
* Deadlock

---

## Phase 11 — PL/SQL

* Anonymous Block
* Variables
* Constants
* Conditions
* Loops
* Cursors
* Exception Handling
* Procedure
* Function
* Trigger
* Package
* Dynamic SQL

Oracle's official PL/SQL documentation covers the language fundamentals, data types, control structures, procedures, functions, packages, and triggers that form the foundation of this section.

---

## Phase 12 — Database Performance

* Query Performance
* Execution Plan
* `EXPLAIN PLAN`
* Indexing
* Full Table Scan
* Index Scan
* Join Strategy
* Statistics
* Query Optimization
* SQL Tuning

The Oracle documentation provides dedicated references for database concepts, administration, and performance-related areas, which will be used as the technical foundation for this section.

---

## Phase 13 — Database Security

* User
* Role
* Privilege
* `GRANT`
* `REVOKE`
* Least Privilege
* Authentication
* Authorization
* SQL Injection
* Parameterized Query
* Secure Database Access

Security will not be treated as an optional topic. OWASP identifies SQL injection as a major application security concern and recommends keeping data separate from SQL commands through safe APIs and parameterized queries.

---

# Reference Standards

Materi dalam repository ini akan mengutamakan **primary sources** dan dokumentasi resmi.

## Tier 01 — Official Oracle Documentation

Primary reference:

* **Oracle AI Database 26ai SQL Language Reference**
* **Oracle AI Database 26ai Database Administrator's Guide**
* **Oracle AI Database 26ai PL/SQL Language Reference**
* **Oracle AI Database 26ai SQL Language Quick Reference**
* **Oracle Database Concepts**

Oracle's current SQL Language Reference documents the SQL language for Oracle AI Database 26ai, while the SQL Quick Reference provides a compact reference for SQL syntax.

[Oracle AI Database 26ai Documentation](https://docs.oracle.com/en/database/oracle/oracle-database/26/?utm_source=chatgpt.com)

---

## Tier 02 — Oracle Learning Resources

Untuk pembelajaran terstruktur, repository ini juga akan merujuk pada:

* Oracle Academy
* Oracle University
* Oracle Dev Gym
* Oracle SQL learning paths

Oracle Academy memiliki curriculum database yang mencakup database design, SQL, dan PL/SQL dengan pendekatan hands-on.

Oracle Dev Gym juga menyediakan course **Databases for Developers: Foundations**, yang berfokus pada database concepts dan SQL dengan Oracle Database.

[Oracle Academy](https://academy.oracle.com/?utm_source=chatgpt.com)

[Oracle Dev Gym — Databases for Developers](https://devgym.oracle.com/pls/apex/dg/class/databases-for-developers-foundations.html?utm_source=chatgpt.com)

---

## Tier 03 — Security Standards

Untuk database security dan secure SQL development:

**OWASP**

Terutama:

* SQL Injection Prevention
* Query Parameterization
* Injection Prevention
* OWASP Top 10

[OWASP SQL Injection Prevention Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/SQL_Injection_Prevention_Cheat_Sheet.html?utm_source=chatgpt.com)

---

# Industry Alignment

Repository ini tidak dibuat untuk mengajarkan SQL dengan pendekatan:

```text
"Hafalkan syntax → kerjakan soal → selesai."
```

Pendekatan yang digunakan:

```text
Database Knowledge
        +
SQL Proficiency
        +
Database Design
        +
Problem Solving
        +
Performance Awareness
        +
Security Awareness
        +
Application Integration
```

Target akhirnya adalah memahami bagaimana SQL digunakan dalam ekosistem software dan data secara nyata.

Contohnya:

```text
Application
     │
     ▼
Backend / API
     │
     ▼
SQL
     │
     ▼
Oracle Database
     │
     ├── Tables
     ├── Indexes
     ├── Transactions
     ├── Constraints
     └── Security
```

Dengan demikian, SQL dipelajari bukan sebagai bahasa yang berdiri sendiri, tetapi sebagai bagian dari **database engineering dan software development**.

---

# Practical Environment

Materi utama menggunakan:

```text
Database      : Oracle Database
SQL           : Oracle SQL
Procedural    : PL/SQL
IDE           : Oracle SQL Developer
Practice      : Oracle FreeSQL / Oracle Database
Version Focus : Oracle AI Database 26ai
```

Oracle juga menyediakan **FreeSQL**, sebuah platform berbasis browser untuk menjalankan dan mempraktikkan Oracle SQL pada database Oracle nyata tanpa perlu melakukan instalasi lokal.

[Oracle FreeSQL](https://www.oracle.com/database/technologies/oracle-free-sql/?utm_source=chatgpt.com)

---

# Repository Structure

Struktur repository akan berkembang mengikuti materi:

```text
oracle-sql/
│
├── README.md
│
├── 01-database-fundamentals/
│   ├── glossary.md
│   ├── dbms-rdbms.md
│   ├── relational-model.md
│   └── database-concepts.md
│
├── 02-database-design/
│   ├── erd.md
│   ├── keys.md
│   ├── relationships.md
│   └── normalization.md
│
├── 03-oracle-fundamentals/
│   ├── oracle-database.md
│   ├── schema.md
│   └── data-types.md
│
├── 04-sql-fundamentals/
│   ├── ddl/
│   ├── dml/
│   ├── dql/
│   ├── tcl/
│   └── dcl/
│
├── 05-functions/
│   ├── character.md
│   ├── number.md
│   ├── date.md
│   ├── conversion.md
│   └── aggregate.md
│
├── 06-joins/
│
├── 07-subqueries/
│
├── 08-advanced-sql/
│
├── 09-database-objects/
│
├── 10-transactions/
│
├── 11-plsql/
│
├── 12-performance/
│
├── 13-security/
│
└── exercises/
    ├── beginner/
    ├── intermediate/
    └── advanced/
```

---

# Documentation Style

Setiap materi akan menggunakan format:

```text
# Concept

## Definition

Apa konsep tersebut?

## Why It Matters

Mengapa konsep tersebut penting?

## Syntax

Bagaimana syntax Oracle SQL-nya?

## Example

Contoh sederhana.

## Real-World Example

Contoh kasus yang lebih realistis.

## Common Mistakes

Kesalahan yang sering dilakukan.

## Best Practices

Praktik yang disarankan.

## Notes

Catatan penting mengenai Oracle.

## Exercises

Latihan untuk menguji pemahaman.
```

Dengan format ini, setiap halaman dapat berfungsi sebagai **reference document**, bukan hanya catatan sekali baca.

---

# Versioning & Accuracy

Database technology terus berkembang.

Karena itu, repository ini akan mencatat:

* Oracle version
* perubahan syntax
* deprecated features
* new features
* compatibility notes
* perbedaan dengan DBMS lain

Fokus utama saat ini adalah **Oracle AI Database 26ai**, yang merupakan rilis Oracle Database terbaru yang didokumentasikan Oracle pada 2026.

Materi yang bersifat fundamental akan tetap dijelaskan berdasarkan konsep database yang stabil, sementara fitur spesifik Oracle akan diberi keterangan versi jika diperlukan.

---

# Learning Status

```text
[████████░░░░░░░░░░░░] In Progress
```

| Area                  | Status      |
| --------------------- | ----------- |
| Database Fundamentals | In Progress |
| Database Design       | Planned     |
| Oracle Fundamentals   | Planned     |
| SQL Fundamentals      | In Progress |
| SQL Functions         | Planned     |
| JOIN                  | Planned     |
| Subquery              | Planned     |
| Advanced SQL          | Planned     |
| Analytic Functions    | Planned     |
| PL/SQL                | Planned     |
| Performance           | Planned     |
| Security              | Planned     |

---

# Goals

By completing this repository, the goal is to be able to:

* Understand relational database concepts.
* Design a relational database.
* Write Oracle SQL confidently.
* Query multiple tables using JOIN.
* Solve problems using subqueries and CTEs.
* Use aggregate and analytic functions.
* Manage database objects.
* Understand transactions and concurrency.
* Write basic to advanced PL/SQL.
* Analyze query performance.
* Apply database security principles.
* Build database solutions that are maintainable and production-aware.

---

# Author's Note

> This repository is not intended to represent the fastest way to learn SQL.
>
> It represents my way of understanding it.
>
> Every concept documented here is part of a longer process: learning, practicing, making mistakes, researching, and refining the understanding.
>
> The objective is not simply to become someone who can write SQL.
>
> The objective is to understand **how data systems work**.

---

## Author

**Sulaimandvlpr**

Learning Database Engineering, Oracle SQL, PL/SQL, and Data Technology.

```text
Learn deeply.
Build consistently.
Document everything.
```

---

## License

This repository is a personal learning project.

The original notes, explanations, examples, and exercises are written and maintained by **Sulaimandvlpr**.

Oracle documentation and other external references remain the property of their respective owners.
