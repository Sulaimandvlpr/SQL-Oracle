Aggregate functions digunakan untuk melakukan perhitungan terhadap **sekumpulan row**.

Function utama:

```text
COUNT()
SUM()
AVG()
MIN()
MAX()
```

---

## COUNT()

Menghitung jumlah row.

```sql
SELECT COUNT(*)
FROM employees;
```

### COUNT(column)

```sql
SELECT COUNT(commission_pct)
FROM employees;
```

Perhatikan bahwa `COUNT(column)` tidak menghitung nilai `NULL`.

---

## SUM()

Menghitung total nilai numerik.

```sql
SELECT SUM(salary)
FROM employees;
```

---

## AVG()

Menghitung nilai rata-rata.

```sql
SELECT AVG(salary)
FROM employees;
```

---

## MIN()

Mencari nilai terkecil.

```sql
SELECT MIN(salary)
FROM employees;
```

---

## MAX()

Mencari nilai terbesar.

```sql
SELECT MAX(salary)
FROM employees;
```

---

# Aggregate Function + GROUP BY

Aggregate function akan menjadi jauh lebih berguna ketika dikombinasikan dengan `GROUP BY`.

Contoh:

```sql
SELECT department_id,
       AVG(salary) AS average_salary
FROM employees
GROUP BY department_id;
```

Konsepnya:

```text
EMPLOYEES
    ↓
GROUP BY department
    ↓
AVG(salary)
    ↓
Average salary setiap department
```

Ini akan menjadi dasar untuk pembahasan **GROUP BY dan HAVING** pada phase berikutnya.

---

# Single-Row vs Aggregate Function

Perbedaan ini **wajib dipahami**.

| Single-Row Function             | Aggregate Function                   |
| ------------------------------- | ------------------------------------ |
| Memproses setiap row            | Memproses sekumpulan row             |
| Menghasilkan satu nilai per row | Menghasilkan satu nilai per kelompok |
| `UPPER()`                       | `COUNT()`                            |
| `LOWER()`                       | `SUM()`                              |
| `LENGTH()`                      | `AVG()`                              |
| `ROUND()`                       | `MIN()`                              |
| `TO_CHAR()`                     | `MAX()`                              |

Contoh:

### Single-row

```sql
SELECT nama,
       UPPER(nama)
FROM mahasiswa;
```

```text
Budi → BUDI
Andi → ANDI
Siti → SITI
```

### Aggregate

```sql
SELECT AVG(umur)
FROM mahasiswa;
```

```text
20
21
19
22
 ↓
20.5
```

---

# Function Nesting

Oracle memungkinkan function digunakan di dalam function lainnya.

Contoh:

```sql
SELECT UPPER(
           TRIM(nama)
       )
FROM mahasiswa;
```

Prosesnya:

```text
nama
 ↓
TRIM()
 ↓
UPPER()
 ↓
hasil
```

Contoh lainnya:

```sql
SELECT ROUND(
           AVG(salary),
           2
       )
FROM employees;
```

Proses:

```text
salary
   ↓
 AVG()
   ↓
 ROUND()
   ↓
hasil
```

Kemampuan menggabungkan function seperti ini akan sangat sering digunakan dalam query nyata.

---

# Real-World Example

Misalkan sebuah perusahaan memiliki data:

```text
EMPLOYEES

EMPLOYEE_ID | FIRST_NAME | SALARY
------------|------------|-------
101         | John       | 5000
102         | Steven     | 6000
103         | Lex        | 7000
```

Kita ingin membuat laporan:

> "Tampilkan nama karyawan dalam uppercase dan gaji tahunan mereka."

Query:

```sql
SELECT UPPER(first_name) AS employee_name,
       salary * 12 AS annual_salary
FROM employees;
```

Hasil:

```text
EMPLOYEE_NAME | ANNUAL_SALARY
--------------|--------------
JOHN          | 60000
STEVEN        | 72000
LEX           | 84000
```

Di sini kita sudah menggabungkan:

```text
UPPER()
+
Arithmetic Operator
+
SELECT
```

Ini merupakan pola yang jauh lebih dekat dengan penggunaan SQL dalam pekerjaan nyata dibandingkan sekadar menjalankan function satu per satu.

---

# Common Mistakes

### 01 — Menggunakan `=` untuk NULL

Salah:

```sql
WHERE commission_pct = NULL;
```

Benar:

```sql
WHERE commission_pct IS NULL;
```

---

### 02 — Mengira `TRUNC()` sama dengan `ROUND()`

```text
ROUND(10.56) → 11
TRUNC(10.56) → 10
```

`ROUND()` membulatkan.

`TRUNC()` memotong.

---

### 03 — Lupa format pada TO_DATE()

Misalnya:

```sql
TO_DATE('30-08-2026', 'YYYY-MM-DD')
```

Format tidak sesuai dengan string input.

Lebih tepat:

```sql
TO_DATE('30-08-2026', 'DD-MM-YYYY')
```

---

### 04 — Menganggap COUNT(*) dan COUNT(column) sama

```sql
COUNT(*)
```

menghitung row.

Sedangkan:

```sql
COUNT(column)
```

tidak menghitung nilai `NULL` pada column tersebut.

---

# Best Practices

### 01 — Gunakan Alias

Daripada:

```sql
SELECT UPPER(first_name)
FROM employees;
```

lebih jelas:

```sql
SELECT UPPER(first_name) AS employee_name
FROM employees;
```

---

### 02 — Jangan melakukan transformasi yang tidak diperlukan

Jika data sudah tersimpan dalam format yang benar, tidak perlu melakukan function tambahan tanpa alasan.

Query yang sederhana biasanya lebih mudah dipahami dan dipelihara.

---

### 03 — Perhatikan penggunaan function pada kolom yang difilter

Contoh:

```sql
WHERE UPPER(first_name) = 'JOHN'
```

Penggunaan function pada kolom dapat berpengaruh terhadap pemanfaatan index dalam kondisi tertentu.

Pembahasan ini akan kita dalami nanti pada **Phase Performance & Query Optimization**.

---
