# GLOSARIUM DATABASE & ORACLE SQL

## A. Konsep Dasar Database

### 1. Data

Sekumpulan fakta atau informasi yang dapat disimpan dan diproses oleh sistem.

Contoh:

```text
NIM       : 2507411018
Nama      : Budi
Umur      : 20
```

---

### 2. Database

Kumpulan data yang **terorganisasi dan disimpan secara sistematis** sehingga dapat diakses, dikelola, dan diproses.

Contoh:

```text
Database Akademik
├── MAHASISWA
├── DOSEN
├── JURUSAN
├── MATA_KULIAH
└── NILAI
```

---

### 3. DBMS — Database Management System

Perangkat lunak yang digunakan untuk **membuat, menyimpan, mengelola, dan mengakses database**.

Contoh DBMS:

* Oracle Database
* MySQL
* PostgreSQL
* Microsoft SQL Server
* SQLite

**Sederhananya:**

> Database = tempat menyimpan data
> DBMS = software yang mengelola database

---

### 4. RDBMS — Relational Database Management System

DBMS yang menggunakan **model relasional**, yaitu menyimpan data dalam bentuk **tabel yang dapat saling berhubungan**.

Contoh:

* Oracle Database
* MySQL
* PostgreSQL
* Microsoft SQL Server

Contoh hubungan:

```text
MAHASISWA
    │
    │ id_jurusan
    ▼
JURUSAN
```

---

### 5. Relational Database

Database yang menggunakan **tabel (relation)** untuk menyimpan data dan memungkinkan tabel-tabel tersebut memiliki hubungan satu sama lain.

---

### 6. Table

Struktur database yang digunakan untuk menyimpan data dalam bentuk **baris dan kolom**.

```text
MAHASISWA

NIM          NAMA       UMUR
-----------  ---------  ----
2501         Budi       20
2502         Andi       21
2503         Siti       19
```

---

### 7. Row / Record

Satu **baris data** dalam sebuah tabel.

```text
2501 | Budi | 20
```

Baris tersebut merupakan satu record mahasiswa.

---

### 8. Column / Field

Atribut atau **kolom** yang menentukan jenis informasi yang disimpan.

Contoh:

```text
NIM
NAMA
UMUR
```

---

### 9. Schema

Struktur atau kumpulan objek database yang dimiliki oleh seorang user dalam Oracle, seperti:

* Table
* View
* Index
* Sequence
* Procedure
* Function
* Trigger

Dalam konteks Oracle, **schema sangat penting** karena setiap user biasanya memiliki schema dengan nama yang sama dengan user tersebut.

---

# B. Bahasa & Teknologi Database

### 10. SQL — Structured Query Language

Bahasa yang digunakan untuk **berinteraksi dengan database**, seperti mengambil, menambahkan, mengubah, dan menghapus data serta mengelola struktur database.

Contoh:

```sql
SELECT *
FROM mahasiswa;
```

---

### 11. Oracle Database

Sistem manajemen database relasional yang dikembangkan oleh **Oracle**.

Oracle Database mendukung SQL serta fitur-fitur database tingkat lanjut.

---

### 12. Oracle SQL

SQL yang digunakan untuk berinteraksi dengan Oracle Database.

Contoh:

```sql
SELECT nama, umur
FROM mahasiswa
WHERE umur >= 20;
```

---

### 13. SQL Developer

Aplikasi dari Oracle yang menyediakan **Graphical User Interface (GUI)** untuk bekerja dengan Oracle Database.

Biasanya digunakan untuk:

* Menulis SQL
* Menjalankan query
* Membuat tabel
* Melihat struktur database
* Mengelola user
* Melihat data

---

### 14. SQL Statement

Satu instruksi SQL yang dikirimkan ke database untuk melakukan suatu operasi.

Contoh:

```sql
SELECT * FROM mahasiswa;
```

---

### 15. SQL Query

Perintah SQL yang digunakan untuk **meminta atau memproses informasi dari database**.

Contoh:

```sql
SELECT nama
FROM mahasiswa
WHERE umur > 20;
```

Dalam pembelajaran sehari-hari, istilah **query** sering digunakan secara luas untuk menyebut perintah SQL.

---

# C. Struktur Data dalam Database

### 16. Primary Key

Kolom atau kombinasi kolom yang digunakan sebagai **identitas unik setiap record**.

Syarat utamanya:

* Nilainya unik
* Tidak boleh `NULL`

Contoh:

```sql
CREATE TABLE mahasiswa (
    nim NUMBER PRIMARY KEY,
    nama VARCHAR2(100)
);
```

`nim` menjadi identitas unik mahasiswa.

---

### 17. Foreign Key

Kolom yang digunakan untuk membuat **hubungan antara satu tabel dengan tabel lainnya**.

Contoh:

```text
MAHASISWA
---------
nim
nama
id_jurusan  ──────────┐
                      │
                      ▼
JURUSAN
-------
id_jurusan
nama_jurusan
```

---

### 18. Candidate Key

Kolom atau kombinasi kolom yang **berpotensi menjadi Primary Key** karena dapat mengidentifikasi record secara unik.

---

### 19. Composite Key

Primary key yang terdiri dari **lebih dari satu kolom**.

Contoh:

```text
nim + id_mata_kuliah
```

---

### 20. Alternate Key

Candidate key yang **tidak dipilih sebagai Primary Key**.

---

### 21. Constraint

Aturan yang diterapkan pada data untuk menjaga **integritas dan validitas data**.

Constraint Oracle yang umum:

```text
PRIMARY KEY
FOREIGN KEY
UNIQUE
NOT NULL
CHECK
```

---

### 22. NULL

Menunjukkan bahwa suatu nilai **tidak tersedia, belum diketahui, atau tidak memiliki nilai**.

`NULL` **bukan**:

```text
0
''
'NULL'
```

---

### 23. UNIQUE

Constraint yang memastikan nilai dalam suatu kolom tidak memiliki duplikasi.

```sql
email VARCHAR2(100) UNIQUE
```

---

### 24. NOT NULL

Memastikan sebuah kolom harus memiliki nilai.

```sql
nama VARCHAR2(100) NOT NULL
```

---

### 25. CHECK

Constraint untuk memberikan kondisi tertentu pada data.

```sql
umur NUMBER CHECK (umur >= 17)
```

---

### 26. DEFAULT

Memberikan nilai otomatis ketika user tidak memberikan nilai.

```sql
status VARCHAR2(20) DEFAULT 'Aktif'
```

---

# D. Operasi SQL

### 27. DDL — Data Definition Language

Digunakan untuk mengelola **struktur database**.

Perintah utama:

```text
CREATE
ALTER
DROP
TRUNCATE
RENAME
COMMENT
```

---

### 28. DML — Data Manipulation Language

Digunakan untuk memanipulasi **data dalam tabel**.

```text
INSERT
UPDATE
DELETE
MERGE
```

---

### 29. DQL — Data Query Language

Istilah yang sering digunakan untuk operasi **pengambilan data**.

Perintah utamanya:

```text
SELECT
```

> Catatan: DQL bukan klasifikasi yang selalu dipisahkan. Dalam beberapa referensi, `SELECT` dimasukkan ke DML.

---

### 30. TCL — Transaction Control Language

Digunakan untuk mengontrol **transaksi**.

```text
COMMIT
ROLLBACK
SAVEPOINT
```

---

### 31. DCL — Data Control Language

Digunakan untuk mengatur **hak akses database**.

```text
GRANT
REVOKE
```

---

# E. Query & Klausa

### 32. SELECT

Mengambil data dari database.

### 33. FROM

Menentukan tabel atau sumber data.

### 34. WHERE

Memberikan kondisi untuk memfilter data.

### 35. DISTINCT

Menghilangkan hasil yang duplikat.

### 36. ORDER BY

Mengurutkan hasil query.

### 37. GROUP BY

Mengelompokkan data.

### 38. HAVING

Memfilter hasil pengelompokan `GROUP BY`.

### 39. JOIN

Menggabungkan data dari beberapa tabel berdasarkan hubungan tertentu.

Jenis umum:

```text
INNER JOIN
LEFT JOIN
RIGHT JOIN
FULL OUTER JOIN
CROSS JOIN
```

### 40. Subquery

Query yang berada di dalam query lain.

### 41. Alias

Nama sementara yang diberikan kepada tabel atau kolom.

```sql
SELECT m.nama
FROM mahasiswa m;
```

`m` adalah alias dari `mahasiswa`.

---

# F. Fungsi Database

### 42. Function

Fungsi yang menerima input dan menghasilkan nilai.

Contoh Oracle:

```sql
UPPER()
LOWER()
LENGTH()
ROUND()
NVL()
```

---

### 43. Aggregate Function

Fungsi yang melakukan perhitungan terhadap sekumpulan baris.

```text
COUNT()
SUM()
AVG()
MIN()
MAX()
```

Contoh:

```sql
SELECT AVG(umur)
FROM mahasiswa;
```

---

### 44. Scalar Function

Fungsi yang bekerja pada nilai/baris secara individual dan menghasilkan satu nilai untuk setiap input.

---

# G. Objek Database Oracle

### 45. View

Tabel virtual yang berasal dari hasil query.

```sql
CREATE VIEW mahasiswa_aktif AS
SELECT *
FROM mahasiswa
WHERE status = 'Aktif';
```

---

### 46. Index

Struktur yang digunakan database untuk membantu **mempercepat pencarian data**.

```sql
CREATE INDEX idx_mahasiswa_nama
ON mahasiswa(nama);
```

---

### 47. Sequence

Objek Oracle yang digunakan untuk menghasilkan **angka secara berurutan**, sering digunakan untuk ID.

```sql
CREATE SEQUENCE seq_mahasiswa
START WITH 1
INCREMENT BY 1;
```

Mengambil nilai:

```sql
seq_mahasiswa.NEXTVAL
```

---

### 48. Synonym

Nama alternatif/alias untuk objek database.

---

### 49. Trigger

Program yang secara otomatis dijalankan ketika terjadi event tertentu pada database.

Contoh event:

```text
INSERT
UPDATE
DELETE
```

---

### 50. Procedure

Program tersimpan di database yang dapat menjalankan sekumpulan instruksi.

---

### 51. Function

Program tersimpan yang dapat menerima parameter dan **mengembalikan sebuah nilai**.

---

### 52. Package

Kumpulan procedure, function, variable, cursor, dan elemen PL/SQL lainnya yang dikelompokkan menjadi satu unit.

---

# H. Transaction & Data Integrity

### 53. Transaction

Sekumpulan operasi database yang diperlakukan sebagai **satu kesatuan pekerjaan**.

Contoh transfer uang:

```text
1. Kurangi saldo A
2. Tambahkan saldo B
```

Keduanya harus berhasil atau perubahan dibatalkan.

---

### 54. COMMIT

Menyimpan perubahan transaksi secara permanen.

```sql
COMMIT;
```

---

### 55. ROLLBACK

Membatalkan perubahan yang belum di-commit.

```sql
ROLLBACK;
```

---

### 56. SAVEPOINT

Membuat titik tertentu dalam transaksi yang dapat digunakan sebagai tujuan rollback.

```sql
SAVEPOINT titik1;
```

---

### 57. ACID

Empat prinsip penting dalam transaksi database:

**A — Atomicity**
Transaksi dilakukan secara utuh atau tidak dilakukan sama sekali.

**C — Consistency**
Transaksi menjaga database tetap dalam kondisi valid.

**I — Isolation**
Transaksi yang berjalan secara bersamaan tidak saling mengganggu secara tidak semestinya.

**D — Durability**
Perubahan yang sudah di-commit tetap tersimpan.

---

# I. Konsep Database Lanjutan

### 58. Data Integrity

Kondisi ketika data tetap **akurat, konsisten, dan valid**.

### 59. Referential Integrity

Menjamin hubungan antara **Primary Key dan Foreign Key** tetap valid.

### 60. Normalization

Proses mengorganisasi data untuk **mengurangi redundansi dan mencegah anomali data**.

Tingkat yang umum:

```text
1NF
2NF
3NF
BCNF
```

### 61. Redundancy

Pengulangan data yang tidak diperlukan.

### 62. Anomaly

Masalah yang dapat terjadi akibat desain database yang buruk.

Umumnya:

```text
Insert Anomaly
Update Anomaly
Delete Anomaly
```

---

# J. Istilah yang Akan Sering Muncul

Beberapa istilah yang juga sebaiknya masuk ke glosariummu:

| Istilah               | Pengertian singkat                                                  |
| --------------------- | ------------------------------------------------------------------- |
| **Entity**            | Objek yang datanya disimpan                                         |
| **Attribute**         | Karakteristik dari entity                                           |
| **Relationship**      | Hubungan antar-entity                                               |
| **Cardinality**       | Jumlah hubungan antar-entity                                        |
| **ERD**               | Entity Relationship Diagram                                         |
| **Schema**            | Struktur/kumpulan objek database                                    |
| **Instance**          | Kondisi data database pada suatu waktu                              |
| **Metadata**          | Data yang menjelaskan data                                          |
| **Data Type**         | Tipe data yang menentukan jenis nilai                               |
| **VARCHAR2**          | Tipe data string Oracle                                             |
| **NUMBER**            | Tipe data numerik Oracle                                            |
| **DATE**              | Tipe data tanggal dan waktu Oracle                                  |
| **TIMESTAMP**         | Tipe data tanggal/waktu dengan presisi lebih tinggi                 |
| **Privilege**         | Hak akses terhadap objek/database                                   |
| **User**              | Akun yang digunakan untuk mengakses database                        |
| **Role**              | Kumpulan privilege                                                  |
| **Schema Object**     | Objek yang dimiliki schema                                          |
| **Cursor**            | Mekanisme untuk memproses hasil query baris demi baris dalam PL/SQL |
| **PL/SQL**            | Bahasa prosedural milik Oracle yang memperluas SQL                  |
| **Optimizer**         | Komponen yang menentukan execution plan query                       |
| **Execution Plan**    | Rencana yang digunakan database untuk menjalankan query             |
| **Query Performance** | Efisiensi sebuah query ketika dijalankan                            |
| **Deadlock**          | Kondisi ketika transaksi saling menunggu resource                   |
| **Lock**              | Mekanisme untuk mengontrol akses bersamaan terhadap data            |

