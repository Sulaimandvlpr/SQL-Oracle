# Kumpulan Kosakata SQL untuk Ujian
## Oracle Academy - Database Programming with SQL

Catatan ini disusun sebagai pendamping `README.md` utama. Fokusnya adalah istilah, konsep, fungsi, operator, objek database, dan keyword yang berpotensi muncul dalam soal pilihan ganda maupun studi kasus.

Gunakan `Ctrl + F` untuk mencari istilah.

---

# 1. Dasar Database dan SQL

## Database
Kumpulan data yang disimpan secara terstruktur agar dapat diakses, dikelola, dan dimanipulasi.

## Relational Database
Database yang menyimpan data dalam bentuk tabel yang saling berhubungan melalui key.

## Table
Objek database yang menyimpan data dalam bentuk baris dan kolom.

```sql
SELECT *
FROM employees;
```

## Row
Satu record lengkap dalam sebuah tabel.

Contoh: satu baris pada tabel `employees` mewakili satu employee.

## Column
Atribut atau karakteristik yang disimpan dalam tabel.

Contoh:
- `employee_id`
- `first_name`
- `salary`
- `department_id`

## Field
Nilai pada perpotongan satu baris dan satu kolom.

## Record
Sekumpulan field yang membentuk satu baris data.

## Primary Key
Kolom atau kombinasi kolom yang mengidentifikasi setiap baris secara unik.

Sifat utama:
- harus unik
- tidak boleh `NULL`
- satu tabel hanya memiliki satu primary key

```sql
CREATE TABLE students (
    student_id NUMBER PRIMARY KEY,
    student_name VARCHAR2(50)
);
```

## Foreign Key
Kolom yang menghubungkan tabel child dengan primary key atau unique key pada tabel parent.

```sql
CREATE TABLE employees_copy (
    employee_id NUMBER PRIMARY KEY,
    department_id NUMBER,
    CONSTRAINT emp_dept_fk
        FOREIGN KEY (department_id)
        REFERENCES departments(department_id)
);
```

## Parent Table
Tabel yang key-nya direferensikan oleh foreign key dari tabel lain.

## Child Table
Tabel yang memiliki foreign key yang mereferensikan tabel parent.

## SQL
Structured Query Language. Bahasa yang digunakan untuk mengakses dan memanipulasi relational database.

## Query
Pernyataan SQL yang digunakan untuk meminta data atau melakukan operasi tertentu.

## Oracle APEX
Lingkungan berbasis web dari Oracle yang dapat digunakan untuk menjalankan SQL dan membangun aplikasi database.

---

# 2. SELECT dan Pengambilan Data

## SELECT
Digunakan untuk menentukan kolom atau ekspresi yang ingin ditampilkan.

```sql
SELECT first_name, last_name
FROM employees;
```

## FROM
Menentukan tabel sumber data.

```sql
SELECT *
FROM employees;
```

## SELECT *
Menampilkan semua kolom dari tabel.

```sql
SELECT *
FROM employees;
```

## Projection
Memilih kolom tertentu yang ingin ditampilkan.

```sql
SELECT first_name, salary
FROM employees;
```

## Selection
Memilih baris tertentu menggunakan kondisi.

```sql
SELECT *
FROM employees
WHERE department_id = 80;
```

## DISTINCT
Menghilangkan baris duplikat dari hasil query.

```sql
SELECT DISTINCT department_id
FROM employees;
```

## Alias
Nama sementara untuk kolom atau tabel.

```sql
SELECT salary AS monthly_salary
FROM employees;
```

Alias tabel:

```sql
SELECT e.last_name, e.salary
FROM employees e;
```

## AS
Keyword opsional untuk memberi alias kolom.

```sql
SELECT salary AS employee_salary
FROM employees;
```

## Literal
Nilai tetap yang ditulis langsung dalam query.

```sql
SELECT last_name, 'Employee'
FROM employees;
```

## Concatenation
Menggabungkan teks atau kolom menggunakan operator `||`.

```sql
SELECT first_name || ' ' || last_name AS full_name
FROM employees;
```

## Arithmetic Expression
Perhitungan matematika di dalam query.

```sql
SELECT salary, salary * 12 AS annual_salary
FROM employees;
```

Operator:
- `+`
- `-`
- `*`
- `/`

## NULL
Menandakan nilai tidak diketahui, tidak tersedia, atau belum diisi.

`NULL` bukan:
- angka 0
- string kosong yang bermakna
- spasi

Contoh:

```sql
SELECT *
FROM employees
WHERE commission_pct IS NULL;
```

---

# 3. WHERE dan Operator Perbandingan

## WHERE
Membatasi baris berdasarkan kondisi.

```sql
SELECT *
FROM employees
WHERE salary > 5000;
```

## =
Sama dengan.

```sql
WHERE department_id = 80
```

## >
Lebih besar dari.

```sql
WHERE salary > 5000
```

## <
Lebih kecil dari.

```sql
WHERE salary < 5000
```

## >=
Lebih besar atau sama dengan.

```sql
WHERE salary >= 5000
```

## <=
Lebih kecil atau sama dengan.

```sql
WHERE salary <= 5000
```

## <>
Tidak sama dengan.

```sql
WHERE department_id <> 80
```

Di Oracle juga sering dijumpai:

```sql
WHERE department_id != 80
```

## BETWEEN
Menguji apakah nilai berada di antara dua batas, termasuk kedua batas.

```sql
SELECT *
FROM employees
WHERE salary BETWEEN 5000 AND 10000;
```

Setara secara logika dengan:

```sql
WHERE salary >= 5000
AND salary <= 10000
```

## NOT BETWEEN
Mencari nilai di luar rentang.

```sql
WHERE salary NOT BETWEEN 5000 AND 10000
```

## IN
Menguji apakah nilai termasuk salah satu nilai dalam daftar.

```sql
WHERE department_id IN (10, 20, 30)
```

## NOT IN
Mencari nilai yang tidak termasuk daftar.

```sql
WHERE department_id NOT IN (10, 20, 30)
```

Perhatian: keberadaan `NULL` pada hasil subquery dapat memengaruhi hasil `NOT IN`.

## LIKE
Pencocokan pola teks.

```sql
WHERE last_name LIKE 'S%'
```

## %
Wildcard untuk nol atau lebih karakter.

```sql
WHERE last_name LIKE 'S%'
```

Artinya: dimulai dengan `S`.

```sql
WHERE last_name LIKE '%son'
```

Artinya: diakhiri `son`.

```sql
WHERE last_name LIKE '%an%'
```

Artinya: mengandung `an`.

## _
Wildcard untuk tepat satu karakter.

```sql
WHERE last_name LIKE '_o%'
```

Artinya: karakter kedua adalah `o`.

## ESCAPE
Menentukan karakter escape agar `%` atau `_` diperlakukan sebagai karakter biasa.

```sql
SELECT last_name, job_id
FROM employees
WHERE job_id LIKE '%\_R%' ESCAPE '\';
```

## IS NULL
Menguji apakah nilai adalah `NULL`.

```sql
WHERE commission_pct IS NULL
```

## IS NOT NULL
Menguji apakah nilai tidak `NULL`.

```sql
WHERE commission_pct IS NOT NULL
```

Salah:

```sql
WHERE commission_pct = NULL
```

Benar:

```sql
WHERE commission_pct IS NULL
```

---

# 4. Operator Logika dan Precedence

## AND
Semua kondisi harus benar.

```sql
SELECT *
FROM employees
WHERE department_id = 80
AND salary > 5000;
```

## OR
Minimal satu kondisi harus benar.

```sql
SELECT *
FROM employees
WHERE department_id = 80
OR department_id = 90;
```

## NOT
Membalik kondisi.

```sql
WHERE NOT department_id = 80
```

## Operator Precedence
Urutan evaluasi operator.

Secara praktis:
1. operasi aritmetika
2. concatenation
3. operator perbandingan
4. `IS NULL`, `LIKE`, `IN`
5. `BETWEEN`
6. `NOT`
7. `AND`
8. `OR`

Karena `AND` dievaluasi sebelum `OR`, gunakan tanda kurung bila logika soal harus dipastikan.

```sql
WHERE (department_id = 50 OR department_id = 80)
AND salary > 5000
```

## Parentheses
Memaksa bagian tertentu dievaluasi lebih dahulu.

```sql
WHERE (department_id = 50 OR department_id = 80)
AND last_name LIKE '%s%'
```

---

# 5. ORDER BY

## ORDER BY
Mengurutkan hasil query.

```sql
SELECT last_name, salary
FROM employees
ORDER BY salary;
```

## ASC
Ascending. Urutan menaik.

```sql
ORDER BY salary ASC;
```

`ASC` adalah default.

## DESC
Descending. Urutan menurun.

```sql
ORDER BY salary DESC;
```

## Multiple-Column Sorting
Mengurutkan berdasarkan lebih dari satu kolom.

```sql
SELECT department_id, last_name
FROM employees
ORDER BY department_id ASC, last_name ASC;
```

## ORDER BY Alias
Hasil dapat diurutkan berdasarkan alias kolom.

```sql
SELECT salary * 12 AS annual_salary
FROM employees
ORDER BY annual_salary DESC;
```

---

# 6. Single-Row Functions

## Single-Row Function
Fungsi yang menerima satu baris dan menghasilkan satu nilai untuk setiap baris.

Kategori utama:
- character
- number
- date
- conversion
- general/null

---

# 7. Character Functions

## UPPER
Mengubah teks menjadi huruf besar.

```sql
SELECT UPPER(last_name)
FROM employees;
```

## LOWER
Mengubah teks menjadi huruf kecil.

```sql
SELECT LOWER(last_name)
FROM employees;
```

## INITCAP
Mengubah huruf pertama setiap kata menjadi kapital.

```sql
SELECT INITCAP('oracle academy')
FROM dual;
```

Hasil:

```text
Oracle Academy
```

## CONCAT
Menggabungkan dua string.

```sql
SELECT CONCAT(first_name, last_name)
FROM employees;
```

Alternatif yang lebih fleksibel:

```sql
SELECT first_name || ' ' || last_name
FROM employees;
```

## SUBSTR
Mengambil sebagian karakter.

```sql
SELECT SUBSTR(last_name, 1, 3)
FROM employees;
```

Artinya: mulai posisi 1, ambil 3 karakter.

## LENGTH
Menghitung jumlah karakter.

```sql
SELECT last_name, LENGTH(last_name)
FROM employees;
```

## INSTR
Mencari posisi suatu karakter/string.

```sql
SELECT INSTR('DATABASE', 'A')
FROM dual;
```

## LPAD
Menambahkan karakter di sebelah kiri sampai panjang tertentu.

```sql
SELECT LPAD(salary, 10, '*')
FROM employees;
```

## RPAD
Menambahkan karakter di sebelah kanan.

```sql
SELECT RPAD(last_name, 15, '.')
FROM employees;
```

## TRIM
Menghapus karakter tertentu dari awal dan/atau akhir string.

```sql
SELECT TRIM('   ORACLE   ')
FROM dual;
```

## REPLACE
Mengganti substring.

```sql
SELECT REPLACE('JACK and JUE', 'J', 'BL')
FROM dual;
```

---

# 8. Number Functions

## ROUND
Membulatkan angka.

```sql
SELECT ROUND(45.926, 2)
FROM dual;
```

Hasil:

```text
45.93
```

## TRUNC
Memotong angka tanpa pembulatan.

```sql
SELECT TRUNC(45.926, 2)
FROM dual;
```

Hasil:

```text
45.92
```

## MOD
Menghasilkan sisa pembagian.

```sql
SELECT MOD(10, 3)
FROM dual;
```

Hasil:

```text
1
```

---

# 9. Date Functions

## DATE
Tipe data untuk menyimpan tanggal dan waktu.

## SYSDATE
Menghasilkan tanggal dan waktu saat ini dari server database.

```sql
SELECT SYSDATE
FROM dual;
```

## Date Arithmetic
Tanggal dapat ditambah atau dikurangi angka.

```sql
SELECT SYSDATE + 7
FROM dual;
```

Artinya tujuh hari setelah hari ini.

```sql
SELECT SYSDATE - 7
FROM dual;
```

Artinya tujuh hari sebelumnya.

## MONTHS_BETWEEN
Menghitung selisih bulan antara dua tanggal.

```sql
SELECT MONTHS_BETWEEN(SYSDATE, hire_date)
FROM employees;
```

## ADD_MONTHS
Menambah sejumlah bulan.

```sql
SELECT ADD_MONTHS(SYSDATE, 3)
FROM dual;
```

## NEXT_DAY
Menghasilkan tanggal hari tertentu berikutnya.

```sql
SELECT NEXT_DAY(SYSDATE, 'MONDAY')
FROM dual;
```

## LAST_DAY
Menghasilkan tanggal terakhir dalam bulan.

```sql
SELECT LAST_DAY(SYSDATE)
FROM dual;
```

## ROUND pada DATE
Membulatkan tanggal ke unit tertentu.

```sql
SELECT ROUND(SYSDATE, 'MONTH')
FROM dual;
```

## TRUNC pada DATE
Memotong bagian tanggal ke unit tertentu.

```sql
SELECT TRUNC(SYSDATE, 'MONTH')
FROM dual;
```

---

# 10. Conversion Functions

## Data Type Conversion
Proses mengubah nilai dari satu tipe data ke tipe lain.

## Implicit Conversion
Konversi dilakukan otomatis oleh Oracle.

## Explicit Conversion
Konversi dilakukan secara eksplisit dengan fungsi.

## TO_CHAR
Mengubah angka atau tanggal menjadi karakter.

```sql
SELECT TO_CHAR(SYSDATE, 'DD-MON-YYYY')
FROM dual;
```

```sql
SELECT TO_CHAR(salary, '999,999.00')
FROM employees;
```

## TO_DATE
Mengubah karakter menjadi tanggal.

```sql
SELECT TO_DATE('05-10-2026', 'DD-MM-YYYY')
FROM dual;
```

## TO_NUMBER
Mengubah karakter numerik menjadi angka.

```sql
SELECT TO_NUMBER('1500')
FROM dual;
```

## Format Model
Pola yang menentukan bagaimana angka atau tanggal ditampilkan/dibaca.

Contoh date format:
- `DD`
- `MM`
- `MON`
- `YYYY`
- `YY`
- `RR`

---

# 11. NULL Functions

## NVL
Jika nilai pertama `NULL`, gunakan nilai pengganti.

```sql
SELECT last_name,
       NVL(commission_pct, 0)
FROM employees;
```

## NVL2
Jika expression tidak `NULL`, gunakan nilai kedua; jika `NULL`, gunakan nilai ketiga.

```sql
SELECT last_name,
       NVL2(commission_pct, 'Has Commission', 'No Commission')
FROM employees;
```

## NULLIF
Menghasilkan `NULL` jika dua expression sama.

```sql
SELECT NULLIF(10, 10)
FROM dual;
```

Hasil:

```text
NULL
```

## COALESCE
Mengembalikan nilai pertama yang tidak `NULL`.

```sql
SELECT COALESCE(commission_pct, manager_id, 0)
FROM employees;
```

---

# 12. Conditional Expressions

## CASE
Ekspresi kondisional standar ANSI/ISO SQL.

```sql
SELECT last_name,
       CASE
           WHEN salary >= 10000 THEN 'HIGH'
           WHEN salary >= 5000 THEN 'MEDIUM'
           ELSE 'LOW'
       END AS salary_level
FROM employees;
```

## Simple CASE
Membandingkan satu expression dengan beberapa nilai.

```sql
SELECT department_id,
       CASE department_id
           WHEN 10 THEN 'Administration'
           WHEN 20 THEN 'Marketing'
           ELSE 'Other'
       END AS department_group
FROM employees;
```

## Searched CASE
Setiap kondisi ditulis menggunakan `WHEN`.

```sql
CASE
    WHEN salary > 10000 THEN 'HIGH'
    ELSE 'NORMAL'
END
```

## DECODE
Ekspresi kondisional proprietary Oracle.

```sql
SELECT department_id,
       DECODE(department_id,
              10, 'Administration',
              20, 'Marketing',
              'Other') AS dept_name
FROM employees;
```

Perbedaan penting:
- `CASE` = standar ANSI/ISO SQL
- `DECODE` = proprietary Oracle

---

# 13. JOIN

## JOIN
Menggabungkan data dari dua atau lebih tabel.

## Cartesian Product
Setiap baris dari tabel pertama dipasangkan dengan setiap baris dari tabel kedua.

```sql
SELECT *
FROM employees
CROSS JOIN departments;
```

Biasanya terjadi karena:
- tidak ada kondisi join
- kondisi join salah

## CROSS JOIN
Sintaks ANSI untuk Cartesian product.

```sql
SELECT *
FROM employees
CROSS JOIN departments;
```

## NATURAL JOIN
Menggabungkan tabel berdasarkan semua kolom dengan nama dan tipe data yang sama.

```sql
SELECT first_name, last_name, job_title
FROM employees
NATURAL JOIN jobs;
```

## JOIN USING
Join menggunakan kolom yang namanya sama pada kedua tabel.

```sql
SELECT last_name, department_name
FROM employees
JOIN departments
USING (department_id);
```

## JOIN ON
Join dengan kondisi eksplisit.

```sql
SELECT e.last_name, d.department_name
FROM employees e
JOIN departments d
ON e.department_id = d.department_id;
```

## INNER JOIN
Hanya mengembalikan baris yang memiliki pasangan pada kedua tabel.

```sql
SELECT e.last_name, d.department_name
FROM employees e
INNER JOIN departments d
ON e.department_id = d.department_id;
```

## LEFT OUTER JOIN
Semua baris tabel kiri tetap ditampilkan.

```sql
SELECT e.last_name, d.department_name
FROM employees e
LEFT OUTER JOIN departments d
ON e.department_id = d.department_id;
```

## RIGHT OUTER JOIN
Semua baris tabel kanan tetap ditampilkan.

```sql
SELECT e.last_name, d.department_name
FROM employees e
RIGHT OUTER JOIN departments d
ON e.department_id = d.department_id;
```

## FULL OUTER JOIN
Semua baris dari kedua tabel ditampilkan.

```sql
SELECT e.last_name, d.department_name
FROM employees e
FULL OUTER JOIN departments d
ON e.department_id = d.department_id;
```

## Equijoin
Join yang menggunakan operator `=`.

```sql
SELECT e.last_name, d.department_name
FROM employees e, departments d
WHERE e.department_id = d.department_id;
```

## Nonequijoin
Join yang menggunakan operator selain `=`.

```sql
SELECT e.last_name, e.salary, j.grade_level
FROM employees e
JOIN job_grades j
ON e.salary BETWEEN j.lowest_sal AND j.highest_sal;
```

## Self-Join
Tabel di-join dengan dirinya sendiri.

Contoh employee dan manager:

```sql
SELECT e.last_name AS employee,
       m.last_name AS manager
FROM employees e
LEFT JOIN employees m
ON e.manager_id = m.employee_id;
```

## Table Alias
Nama pendek untuk tabel.

```sql
FROM employees e
JOIN departments d
```

---

# 14. Hierarchical Query

## Hierarchical Query
Query untuk menampilkan hubungan parent-child berbentuk tree.

## START WITH
Menentukan root hierarchy.

```sql
START WITH manager_id IS NULL
```

## CONNECT BY
Menentukan hubungan parent-child.

```sql
CONNECT BY PRIOR employee_id = manager_id
```

## PRIOR
Menandai expression dari baris parent sebelumnya.

## LEVEL
Pseudo-column yang menunjukkan tingkat hierarchy.

Contoh:

```sql
SELECT LEVEL, last_name
FROM employees
START WITH manager_id IS NULL
CONNECT BY PRIOR employee_id = manager_id;
```

---

# 15. Group Functions

## Group Function
Fungsi yang bekerja pada sekumpulan baris dan menghasilkan satu nilai per grup.

## SUM
Menjumlahkan nilai.

```sql
SELECT SUM(salary)
FROM employees;
```

## AVG
Menghitung rata-rata.

```sql
SELECT AVG(salary)
FROM employees;
```

## COUNT
Menghitung jumlah nilai non-NULL.

```sql
SELECT COUNT(commission_pct)
FROM employees;
```

## COUNT(*)
Menghitung seluruh baris, termasuk baris yang memiliki `NULL` pada kolom tertentu.

```sql
SELECT COUNT(*)
FROM employees;
```

## MIN
Nilai minimum.

```sql
SELECT MIN(salary)
FROM employees;
```

## MAX
Nilai maksimum.

```sql
SELECT MAX(salary)
FROM employees;
```

## STDDEV
Menghitung standard deviation.

```sql
SELECT STDDEV(salary)
FROM employees;
```

## VARIANCE
Menghitung variance.

```sql
SELECT VARIANCE(salary)
FROM employees;
```

---

# 16. GROUP BY dan HAVING

## GROUP BY
Mengelompokkan baris sebelum fungsi grup dihitung.

```sql
SELECT department_id, AVG(salary)
FROM employees
GROUP BY department_id;
```

Aturan penting:
Kolom pada `SELECT` yang bukan fungsi grup harus muncul dalam `GROUP BY`.

## HAVING
Memfilter grup setelah proses grouping.

```sql
SELECT department_id, AVG(salary)
FROM employees
GROUP BY department_id
HAVING AVG(salary) > 8000;
```

## WHERE vs HAVING
`WHERE` memfilter baris sebelum grouping.

`HAVING` memfilter hasil grup sesudah grouping.

```sql
SELECT department_id, AVG(salary)
FROM employees
WHERE salary > 3000
GROUP BY department_id
HAVING AVG(salary) > 5000;
```

## Nested Group Function
Fungsi grup di dalam fungsi grup lain.

```sql
SELECT MAX(AVG(salary))
FROM employees
GROUP BY department_id;
```

---

# 17. ROLLUP, CUBE, dan GROUPING SETS

## ROLLUP
Menghasilkan subtotal secara hierarkis serta grand total.

```sql
SELECT department_id, job_id, SUM(salary)
FROM employees
GROUP BY ROLLUP(department_id, job_id);
```

## CUBE
Menghasilkan subtotal untuk semua kombinasi kolom grouping.

```sql
SELECT department_id, job_id, SUM(salary)
FROM employees
GROUP BY CUBE(department_id, job_id);
```

## GROUPING SETS
Menentukan kombinasi grup yang diinginkan secara eksplisit.

```sql
SELECT department_id, job_id, SUM(salary)
FROM employees
GROUP BY GROUPING SETS (
    (department_id),
    (job_id),
    ()
);
```

## GROUPING
Membedakan `NULL` asli dari `NULL` yang dihasilkan oleh `ROLLUP` atau `CUBE`.

```sql
SELECT department_id,
       GROUPING(department_id),
       SUM(salary)
FROM employees
GROUP BY ROLLUP(department_id);
```

---

# 18. Set Operators

## Set Operator
Menggabungkan hasil dua atau lebih query.

Syarat penting:
- jumlah kolom harus sama
- posisi kolom harus compatible
- tipe data harus compatible

## UNION
Menggabungkan hasil dan menghapus duplikat.

```sql
SELECT department_id
FROM employees
UNION
SELECT department_id
FROM departments;
```

## UNION ALL
Menggabungkan hasil dan mempertahankan duplikat.

```sql
SELECT department_id
FROM employees
UNION ALL
SELECT department_id
FROM departments;
```

## INTERSECT
Menghasilkan baris yang terdapat pada kedua query.

```sql
SELECT department_id
FROM employees
INTERSECT
SELECT department_id
FROM departments;
```

## MINUS
Menghasilkan baris dari query pertama yang tidak ada pada query kedua.

```sql
SELECT department_id
FROM departments
MINUS
SELECT department_id
FROM employees;
```

---

# 19. Subquery

## Subquery
Query yang berada di dalam query lain.

```sql
SELECT last_name, salary
FROM employees
WHERE salary > (
    SELECT AVG(salary)
    FROM employees
);
```

## Outer Query
Query utama yang berisi subquery.

## Inner Query
Query di dalam outer query.

## Single-Row Subquery
Subquery yang menghasilkan satu baris.

Gunakan operator:
- `=`
- `>`
- `<`
- `>=`
- `<=`
- `<>`

```sql
SELECT last_name
FROM employees
WHERE salary > (
    SELECT salary
    FROM employees
    WHERE employee_id = 100
);
```

## Multi-Row Subquery
Subquery yang menghasilkan lebih dari satu baris.

Gunakan:
- `IN`
- `ANY`
- `ALL`

```sql
SELECT last_name
FROM employees
WHERE department_id IN (
    SELECT department_id
    FROM departments
    WHERE location_id = 1700
);
```

## ANY
Kondisi benar jika cocok terhadap minimal satu nilai hasil subquery.

```sql
WHERE salary > ANY (
    SELECT salary
    FROM employees
    WHERE department_id = 80
)
```

`> ANY` berarti lebih besar dari minimal satu nilai.

## ALL
Kondisi harus benar terhadap semua nilai hasil subquery.

```sql
WHERE salary > ALL (
    SELECT salary
    FROM employees
    WHERE department_id = 80
)
```

`> ALL` berarti lebih besar dari seluruh nilai.

## Pair-Wise Subquery
Membandingkan pasangan beberapa kolom secara bersama.

```sql
WHERE (manager_id, department_id) IN (
    SELECT manager_id, department_id
    FROM employees
    WHERE employee_id IN (178, 174)
)
```

## Non-Pair-Wise Subquery
Beberapa kondisi dibandingkan melalui subquery terpisah.

## Correlated Subquery
Subquery yang bergantung pada baris dari outer query dan dievaluasi berulang.

```sql
SELECT e.last_name, e.salary
FROM employees e
WHERE e.salary > (
    SELECT AVG(e2.salary)
    FROM employees e2
    WHERE e2.department_id = e.department_id
);
```

## EXISTS
Bernilai benar jika subquery mengembalikan minimal satu baris.

```sql
SELECT department_name
FROM departments d
WHERE EXISTS (
    SELECT 1
    FROM employees e
    WHERE e.department_id = d.department_id
);
```

## NOT EXISTS
Bernilai benar jika subquery tidak mengembalikan baris.

```sql
SELECT department_name
FROM departments d
WHERE NOT EXISTS (
    SELECT 1
    FROM employees e
    WHERE e.department_id = d.department_id
);
```

## WITH Clause
Membuat named subquery atau common table expression.

```sql
WITH dept_avg AS (
    SELECT department_id,
           AVG(salary) AS avg_salary
    FROM employees
    GROUP BY department_id
)
SELECT *
FROM dept_avg;
```

---

# 20. DML

## DML
Data Manipulation Language.

Digunakan untuk memanipulasi data dalam tabel.

Contoh:
- `INSERT`
- `UPDATE`
- `DELETE`
- `MERGE`

## INSERT
Menambah baris baru.

```sql
INSERT INTO departments (
    department_id,
    department_name
)
VALUES (
    300,
    'Research'
);
```

## INSERT dengan semua kolom
Jika semua kolom dituliskan sesuai urutan tabel:

```sql
INSERT INTO departments
VALUES (300, 'Research', NULL, 1700);
```

## INSERT dengan NULL
```sql
INSERT INTO departments (
    department_id,
    department_name,
    manager_id
)
VALUES (
    300,
    'Research',
    NULL
);
```

## INSERT dengan Subquery
Menyalin data dari tabel lain.

```sql
INSERT INTO emp_copy (
    employee_id,
    first_name,
    last_name
)
SELECT employee_id,
       first_name,
       last_name
FROM employees
WHERE department_id = 80;
```

## UPDATE
Mengubah data yang sudah ada.

```sql
UPDATE employees
SET salary = 7000
WHERE employee_id = 200;
```

Update beberapa kolom:

```sql
UPDATE employees
SET salary = 7000,
    department_id = 80
WHERE employee_id = 200;
```

## DELETE
Menghapus baris.

```sql
DELETE FROM employees
WHERE employee_id = 200;
```

Bahaya:

```sql
DELETE FROM employees;
```

Menghapus seluruh baris.

## MERGE
Menggabungkan proses update dan insert berdasarkan kondisi.

```sql
MERGE INTO destination d
USING source s
ON (d.id = s.id)
WHEN MATCHED THEN
    UPDATE SET d.name = s.name
WHEN NOT MATCHED THEN
    INSERT (id, name)
    VALUES (s.id, s.name);
```

## DEFAULT
Nilai otomatis yang digunakan jika nilai tidak diberikan.

```sql
CREATE TABLE orders (
    order_id NUMBER,
    status VARCHAR2(20) DEFAULT 'NEW'
);
```

## Multi-Table INSERT
Satu query dapat memasukkan data ke beberapa tabel.

```sql
INSERT ALL
    INTO emp_sales VALUES (employee_id, last_name)
    INTO emp_backup VALUES (employee_id, last_name)
SELECT employee_id, last_name
FROM employees
WHERE department_id = 80;
```

---

# 21. DDL

## DDL
Data Definition Language.

Digunakan untuk membuat atau mengubah struktur objek database.

Contoh:
- `CREATE`
- `ALTER`
- `DROP`
- `TRUNCATE`

## CREATE
Membuat objek database.

```sql
CREATE TABLE students (
    student_id NUMBER,
    student_name VARCHAR2(50)
);
```

## ALTER
Mengubah struktur objek.

```sql
ALTER TABLE students
ADD email VARCHAR2(100);
```

## DROP
Menghapus objek database.

```sql
DROP TABLE students;
```

## TRUNCATE
Menghapus seluruh baris tabel dengan cepat tanpa menghapus struktur tabel.

```sql
TRUNCATE TABLE students;
```

---

# 22. CREATE TABLE dan Data Types

## CREATE TABLE
Membuat tabel baru.

```sql
CREATE TABLE students (
    student_id NUMBER(6),
    student_name VARCHAR2(50),
    birth_date DATE
);
```

## NUMBER
Menyimpan nilai numerik.

```sql
salary NUMBER(8,2)
```

`8` = precision total digit.

`2` = scale digit di belakang desimal.

## VARCHAR2
Menyimpan karakter dengan panjang variabel.

```sql
name VARCHAR2(50)
```

Gunakan untuk teks dengan panjang yang berubah-ubah.

## CHAR
Menyimpan karakter dengan panjang tetap.

```sql
gender_code CHAR(1)
```

## DATE
Menyimpan tanggal dan waktu.

```sql
hire_date DATE
```

## TIMESTAMP
Menyimpan tanggal dan waktu dengan fractional seconds.

```sql
created_at TIMESTAMP
```

## TIMESTAMP WITH TIME ZONE
Menyimpan timestamp beserta time zone.

```sql
created_at TIMESTAMP WITH TIME ZONE
```

## TIMESTAMP WITH LOCAL TIME ZONE
Menyimpan nilai yang dinormalisasi ke time zone database dan ditampilkan sesuai session time zone.

## INTERVAL YEAR TO MONTH
Menyimpan interval berdasarkan tahun dan bulan.

## INTERVAL DAY TO SECOND
Menyimpan interval berdasarkan hari, jam, menit, dan detik.

---

# 23. ALTER TABLE

## ADD
Menambahkan kolom.

```sql
ALTER TABLE students
ADD email VARCHAR2(100);
```

## MODIFY
Mengubah definisi kolom.

```sql
ALTER TABLE students
MODIFY student_name VARCHAR2(100);
```

## DROP COLUMN
Menghapus kolom.

```sql
ALTER TABLE students
DROP COLUMN email;
```

## RENAME COLUMN
Mengubah nama kolom.

```sql
ALTER TABLE students
RENAME COLUMN student_name TO full_name;
```

## SET UNUSED
Menandai kolom sebagai tidak digunakan.

```sql
ALTER TABLE students
SET UNUSED COLUMN email;
```

Kolom tidak langsung dihapus secara fisik.

---

# 24. Constraint

## Constraint
Aturan untuk menjaga validitas dan integritas data.

Constraint utama:
- `NOT NULL`
- `UNIQUE`
- `PRIMARY KEY`
- `FOREIGN KEY`
- `CHECK`

## NOT NULL
Kolom wajib memiliki nilai.

```sql
last_name VARCHAR2(50) NOT NULL
```

`NOT NULL` hanya dapat didefinisikan pada level kolom.

## UNIQUE
Mencegah nilai duplikat.

```sql
email VARCHAR2(100) UNIQUE
```

Dengan nama:

```sql
CONSTRAINT students_email_uk UNIQUE (email)
```

## PRIMARY KEY
Unik dan tidak boleh `NULL`.

```sql
student_id NUMBER
    CONSTRAINT students_pk PRIMARY KEY
```

## Composite Primary Key
Primary key yang terdiri dari lebih dari satu kolom.

Harus dibuat pada level tabel.

```sql
CONSTRAINT enrollment_pk
PRIMARY KEY (student_id, course_id)
```

## FOREIGN KEY
Menjaga hubungan dengan tabel lain.

```sql
CONSTRAINT emp_dept_fk
FOREIGN KEY (department_id)
REFERENCES departments(department_id)
```

## REFERENCES
Menentukan tabel dan kolom parent dari foreign key.

```sql
REFERENCES departments(department_id)
```

## CHECK
Membatasi nilai sesuai kondisi.

```sql
salary NUMBER
    CONSTRAINT salary_ck CHECK (salary > 0)
```

## ON DELETE CASCADE
Jika row parent dihapus, row child yang mereferensikannya ikut dihapus.

```sql
FOREIGN KEY (department_id)
REFERENCES departments(department_id)
ON DELETE CASCADE
```

## Referential Integrity
Menjamin foreign key hanya berisi nilai yang valid sesuai parent key, atau `NULL` jika diperbolehkan.

## Composite Key
Key yang terdiri dari lebih dari satu kolom.

## Column-Level Constraint
Constraint ditulis bersama definisi kolom.

```sql
student_id NUMBER PRIMARY KEY
```

## Table-Level Constraint
Constraint ditulis setelah definisi semua kolom.

```sql
CONSTRAINT students_pk PRIMARY KEY (student_id)
```

---

# 25. Managing Constraints

## ADD CONSTRAINT
Menambahkan constraint ke tabel yang sudah ada.

```sql
ALTER TABLE students
ADD CONSTRAINT students_pk
PRIMARY KEY (student_id);
```

## DROP CONSTRAINT
Menghapus constraint.

```sql
ALTER TABLE students
DROP CONSTRAINT students_pk;
```

## DISABLE CONSTRAINT
Menonaktifkan constraint sementara.

```sql
ALTER TABLE students
DISABLE CONSTRAINT students_pk;
```

## ENABLE CONSTRAINT
Mengaktifkan kembali constraint.

```sql
ALTER TABLE students
ENABLE CONSTRAINT students_pk;
```

---

# 26. VIEW

## View
Representasi logis atau virtual dari data pada satu atau lebih tabel.

View tidak menyimpan data sendiri seperti tabel biasa.

## Base Table
Tabel asli yang menjadi sumber view.

## Simple View
View yang umumnya berasal dari satu tabel dan tidak menggunakan group function atau operasi kompleks.

```sql
CREATE VIEW emp80_vu AS
SELECT employee_id, last_name, salary
FROM employees
WHERE department_id = 80;
```

## Complex View
View yang menggunakan beberapa tabel, fungsi grup, atau operasi kompleks.

```sql
CREATE VIEW dept_salary_vu AS
SELECT department_id,
       AVG(salary) AS avg_salary
FROM employees
GROUP BY department_id;
```

## CREATE VIEW
Membuat view.

```sql
CREATE VIEW emp_vu AS
SELECT employee_id, last_name
FROM employees;
```

## CREATE OR REPLACE VIEW
Membuat view baru atau mengganti definisi view yang sudah ada.

```sql
CREATE OR REPLACE VIEW emp_vu AS
SELECT employee_id, last_name, salary
FROM employees;
```

## WITH CHECK OPTION
Mencegah DML melalui view yang menyebabkan row keluar dari kondisi view.

```sql
CREATE VIEW emp80_vu AS
SELECT *
FROM employees
WHERE department_id = 80
WITH CHECK OPTION;
```

## WITH READ ONLY
Membuat view tidak dapat diubah dengan DML.

```sql
CREATE VIEW emp_ro_vu AS
SELECT employee_id, last_name
FROM employees
WITH READ ONLY;
```

## DROP VIEW
Menghapus view.

```sql
DROP VIEW emp_vu;
```

---

# 27. SEQUENCE

## Sequence
Objek database yang menghasilkan nomor unik secara otomatis.

Sering digunakan untuk primary key.

## CREATE SEQUENCE
Membuat sequence.

```sql
CREATE SEQUENCE emp_seq
START WITH 1000
INCREMENT BY 1
NOCACHE
NOCYCLE;
```

## START WITH
Nilai awal sequence.

```sql
START WITH 1000
```

## INCREMENT BY
Besarnya kenaikan.

```sql
INCREMENT BY 1
```

## NEXTVAL
Menghasilkan nilai sequence berikutnya.

```sql
SELECT emp_seq.NEXTVAL
FROM dual;
```

Untuk INSERT:

```sql
INSERT INTO employees_copy (
    employee_id,
    last_name
)
VALUES (
    emp_seq.NEXTVAL,
    'Smith'
);
```

## CURRVAL
Menghasilkan nilai sequence terakhir pada session setelah `NEXTVAL` pernah digunakan.

```sql
SELECT emp_seq.CURRVAL
FROM dual;
```

## CACHE
Menyimpan sejumlah nilai sequence di memory untuk meningkatkan performa.

## NOCACHE
Tidak menyimpan cache nilai sequence.

## CYCLE
Sequence dapat kembali ke nilai awal setelah mencapai batas.

## NOCYCLE
Sequence tidak berulang.

## ALTER SEQUENCE
Mengubah properti sequence.

```sql
ALTER SEQUENCE emp_seq
INCREMENT BY 10;
```

## DROP SEQUENCE
Menghapus sequence.

```sql
DROP SEQUENCE emp_seq;
```

---

# 28. INDEX

## Index
Objek schema yang mempercepat pencarian row menggunakan pointer.

## Full Table Scan
Database membaca seluruh tabel untuk mencari row.

Index dapat mengurangi kebutuhan full table scan untuk query tertentu.

## CREATE INDEX
Membuat index.

```sql
CREATE INDEX emp_last_name_idx
ON employees(last_name);
```

## DROP INDEX
Menghapus index.

```sql
DROP INDEX emp_last_name_idx;
```

## Unique Index
Index yang tidak mengizinkan duplicate key.

## Nonunique Index
Index yang mengizinkan duplicate key.

## Composite Index
Index yang menggunakan lebih dari satu kolom.

```sql
CREATE INDEX emp_dept_job_idx
ON employees(department_id, job_id);
```

## Function-Based Index
Index berdasarkan expression atau fungsi.

```sql
CREATE INDEX upper_last_name_idx
ON employees(UPPER(last_name));
```

Query yang dapat memanfaatkannya:

```sql
SELECT *
FROM employees
WHERE UPPER(last_name) = 'KING';
```

## ROWID
Alamat unik internal dari row di database.

Index menggunakan ROWID untuk menemukan row secara cepat.

---

# 29. SYNONYM

## Synonym
Nama alternatif untuk objek database.

## Private Synonym
Synonym yang tersedia pada schema user tertentu.

```sql
CREATE SYNONYM emp
FOR employees;
```

Pemakaian:

```sql
SELECT *
FROM emp;
```

## Public Synonym
Synonym yang tersedia lebih luas untuk user database.

```sql
CREATE PUBLIC SYNONYM emp_public
FOR employees;
```

## DROP SYNONYM
Menghapus private synonym.

```sql
DROP SYNONYM emp;
```

## DROP PUBLIC SYNONYM
Menghapus public synonym.

```sql
DROP PUBLIC SYNONYM emp_public;
```

---

# 30. Istilah Penting yang Sering Membingungkan pada Pilihan Ganda

## DML vs DDL
DML memanipulasi isi data:

```text
INSERT
UPDATE
DELETE
MERGE
```

DDL memanipulasi struktur objek:

```text
CREATE
ALTER
DROP
TRUNCATE
```

## WHERE vs HAVING
`WHERE` menyaring row.

`HAVING` menyaring group.

## UNIQUE vs PRIMARY KEY
`PRIMARY KEY`:
- unik
- tidak NULL
- hanya satu per tabel

`UNIQUE`:
- menjaga keunikan
- satu tabel dapat memiliki beberapa unique constraint

## DELETE vs DROP vs TRUNCATE
`DELETE`:
- menghapus row
- dapat memakai `WHERE`

```sql
DELETE FROM employees
WHERE employee_id = 200;
```

`TRUNCATE`:
- menghapus semua row
- tidak menggunakan `WHERE`
- struktur tabel tetap ada

```sql
TRUNCATE TABLE employees_copy;
```

`DROP`:
- menghapus objek beserta strukturnya

```sql
DROP TABLE employees_copy;
```

## CHAR vs VARCHAR2
`CHAR`:
- fixed length

`VARCHAR2`:
- variable length

## UNION vs UNION ALL
`UNION` menghapus duplikat.

`UNION ALL` mempertahankan duplikat.

## COUNT(*) vs COUNT(column)
`COUNT(*)` menghitung seluruh row.

`COUNT(column)` mengabaikan `NULL` pada column.

## INNER JOIN vs OUTER JOIN
`INNER JOIN` hanya row yang match.

`OUTER JOIN` juga dapat mempertahankan row yang tidak match.

## NATURAL JOIN vs USING vs ON
`NATURAL JOIN`:
- Oracle menentukan kolom yang sama secara otomatis.

`USING`:
- kita menentukan nama kolom bersama.

`ON`:
- kondisi join ditulis penuh.

## IN vs EXISTS
`IN` membandingkan nilai terhadap sekumpulan nilai.

`EXISTS` hanya memeriksa apakah subquery menghasilkan row.

## ANY vs ALL
`ANY`: cukup memenuhi terhadap minimal satu nilai.

`ALL`: harus memenuhi terhadap semua nilai.

## CASE vs DECODE
`CASE`:
- ANSI/ISO SQL
- lebih fleksibel

`DECODE`:
- proprietary Oracle

## NVL vs COALESCE
`NVL` menggunakan dua argument.

```sql
NVL(value, replacement)
```

`COALESCE` dapat memeriksa beberapa expression.

```sql
COALESCE(a, b, c, 0)
```

## ROUND vs TRUNC
`ROUND` melakukan pembulatan.

`TRUNC` memotong tanpa pembulatan.

## NEXTVAL vs CURRVAL
`NEXTVAL` menghasilkan nilai baru.

`CURRVAL` mengambil nilai terakhir yang telah dihasilkan dalam session.

---

# 31. Kata Kunci Bahasa Soal dan Jawabannya

| Jika soal menyebut | Konsep yang dicari |
|---|---|
| menampilkan data | `SELECT` |
| sumber tabel | `FROM` |
| hanya baris tertentu | `WHERE` |
| semua kolom | `*` |
| tanpa duplikat | `DISTINCT` |
| nama sementara | Alias |
| menggabungkan teks | `||` / `CONCAT` |
| di antara | `BETWEEN` |
| salah satu dari beberapa nilai | `IN` |
| pola teks | `LIKE` |
| diawali | `LIKE 'A%'` |
| diakhiri | `LIKE '%A'` |
| mengandung | `LIKE '%A%'` |
| tepat satu karakter wildcard | `_` |
| nilai kosong/tidak diketahui | `NULL` |
| cek NULL | `IS NULL` |
| semua kondisi benar | `AND` |
| minimal satu kondisi benar | `OR` |
| membalik kondisi | `NOT` |
| urut naik | `ASC` |
| urut turun | `DESC` |
| huruf besar | `UPPER` |
| huruf kecil | `LOWER` |
| kapital awal kata | `INITCAP` |
| mengambil sebagian teks | `SUBSTR` |
| panjang teks | `LENGTH` |
| posisi karakter | `INSTR` |
| mengganti teks | `REPLACE` |
| pembulatan | `ROUND` |
| potong tanpa pembulatan | `TRUNC` |
| sisa pembagian | `MOD` |
| tanggal sekarang | `SYSDATE` |
| ubah date/number ke text | `TO_CHAR` |
| ubah text ke date | `TO_DATE` |
| ubah text ke number | `TO_NUMBER` |
| ganti NULL | `NVL` |
| kondisi if-else | `CASE` |
| kondisi Oracle khusus | `DECODE` |
| gabung dua tabel | `JOIN` |
| gabung tabel ke dirinya sendiri | `SELF JOIN` |
| semua kombinasi row | `CROSS JOIN` / Cartesian product |
| total | `SUM` |
| rata-rata | `AVG` |
| jumlah row | `COUNT` |
| nilai terbesar | `MAX` |
| nilai terkecil | `MIN` |
| kelompok | `GROUP BY` |
| kondisi pada hasil group | `HAVING` |
| subtotal hierarkis | `ROLLUP` |
| semua kombinasi subtotal | `CUBE` |
| kelompok tertentu | `GROUPING SETS` |
| gabung hasil tanpa duplikat | `UNION` |
| gabung hasil dengan duplikat | `UNION ALL` |
| data yang sama di dua query | `INTERSECT` |
| data query pertama yang tidak ada di kedua | `MINUS` |
| query di dalam query | Subquery |
| subquery menghasilkan banyak row | Multi-row subquery |
| lebih besar dari setidaknya satu | `> ANY` |
| lebih besar dari semua | `> ALL` |
| cek apakah subquery menghasilkan row | `EXISTS` |
| tidak ada row dari subquery | `NOT EXISTS` |
| named subquery | `WITH` |
| tambah data | `INSERT` |
| ubah data | `UPDATE` |
| hapus row | `DELETE` |
| update atau insert sekaligus | `MERGE` |
| buat tabel | `CREATE TABLE` |
| ubah struktur tabel | `ALTER TABLE` |
| hapus tabel | `DROP TABLE` |
| kosongkan isi tabel | `TRUNCATE TABLE` |
| wajib diisi | `NOT NULL` |
| harus unik | `UNIQUE` |
| identitas utama row | `PRIMARY KEY` |
| hubungan antar tabel | `FOREIGN KEY` |
| validasi nilai | `CHECK` |
| tabel virtual | `VIEW` |
| auto-number | `SEQUENCE` |
| nilai sequence berikutnya | `NEXTVAL` |
| nilai sequence terakhir | `CURRVAL` |
| mempercepat pencarian | `INDEX` |
| nama alternatif objek | `SYNONYM` |

---

# 32. Pola Soal Pilihan Ganda yang Sangat Mungkin Muncul

## Pertanyaan
Keyword mana yang digunakan untuk membatasi row?

Jawaban:

```text
WHERE
```

## Pertanyaan
Keyword mana yang digunakan untuk membatasi group?

Jawaban:

```text
HAVING
```

## Pertanyaan
Fungsi mana yang menghitung seluruh row termasuk row dengan nilai NULL pada kolom tertentu?

Jawaban:

```text
COUNT(*)
```

## Pertanyaan
Operator apa yang digunakan untuk mencari pola karakter?

Jawaban:

```text
LIKE
```

## Pertanyaan
Wildcard untuk tepat satu karakter?

Jawaban:

```text
_
```

## Pertanyaan
Wildcard untuk nol atau banyak karakter?

Jawaban:

```text
%
```

## Pertanyaan
Fungsi apa yang mengubah teks menjadi uppercase?

Jawaban:

```text
UPPER
```

## Pertanyaan
Fungsi apa yang mengganti NULL dengan nilai tertentu?

Jawaban:

```text
NVL
```

## Pertanyaan
Apa yang menggabungkan data antar tabel?

Jawaban:

```text
JOIN
```

## Pertanyaan
Join apa yang menghasilkan semua kombinasi baris?

Jawaban:

```text
CROSS JOIN
```

## Pertanyaan
Apa fungsi `GROUP BY`?

Jawaban:

```text
Mengelompokkan row agar fungsi grup dapat dihitung per kelompok.
```

## Pertanyaan
Perbedaan `WHERE` dan `HAVING`?

Jawaban:

```text
WHERE memfilter row sebelum grouping.
HAVING memfilter group setelah grouping.
```

## Pertanyaan
Operator set yang mempertahankan duplikat?

Jawaban:

```text
UNION ALL
```

## Pertanyaan
Subquery yang menghasilkan beberapa row menggunakan operator apa?

Jawaban umum:

```text
IN, ANY, ALL
```

## Pertanyaan
Constraint yang menjaga hubungan parent-child?

Jawaban:

```text
FOREIGN KEY
```

## Pertanyaan
Constraint yang menjamin nilai unik dan tidak NULL?

Jawaban:

```text
PRIMARY KEY
```

## Pertanyaan
Objek database untuk nomor unik otomatis?

Jawaban:

```text
SEQUENCE
```

## Pertanyaan
Pseudo-column untuk nilai sequence berikutnya?

Jawaban:

```text
NEXTVAL
```

## Pertanyaan
Objek yang mempercepat retrieval row?

Jawaban:

```text
INDEX
```

## Pertanyaan
Nama alternatif untuk objek database?

Jawaban:

```text
SYNONYM
```

## Pertanyaan
Apakah view menyimpan data sendiri?

Jawaban:

```text
Tidak. View adalah representasi logis dari data pada base table/view lain.
```

---

# 33. Ringkasan Super Singkat

```text
SELECT      = ambil kolom/data
FROM        = sumber tabel
WHERE       = filter row
DISTINCT    = hapus duplikat
ORDER BY    = urutkan
GROUP BY    = kelompokkan
HAVING      = filter group

SUM         = total
AVG         = rata-rata
COUNT       = hitung
MIN         = minimum
MAX         = maksimum

UPPER       = uppercase
LOWER       = lowercase
INITCAP     = kapital awal
SUBSTR      = potong string
LENGTH      = panjang string
INSTR       = posisi karakter
REPLACE     = ganti string

ROUND       = pembulatan
TRUNC       = potong
MOD         = sisa bagi

SYSDATE     = tanggal sekarang
TO_CHAR     = ubah ke text
TO_DATE     = text ke date
TO_NUMBER   = text ke number

NVL         = ganti NULL
CASE        = kondisi standar SQL
DECODE      = kondisi proprietary Oracle

JOIN        = gabung tabel
CROSS JOIN  = Cartesian product
SELF JOIN   = tabel join dirinya sendiri
OUTER JOIN  = pertahankan unmatched row

UNION       = gabung tanpa duplikat
UNION ALL   = gabung dengan duplikat
INTERSECT   = irisan
MINUS       = selisih

SUBQUERY    = query di dalam query
EXISTS      = ada row
NOT EXISTS  = tidak ada row
ANY         = minimal satu
ALL         = semua

INSERT      = tambah row
UPDATE      = ubah row
DELETE      = hapus row
MERGE       = update/insert

CREATE      = buat objek
ALTER       = ubah struktur
DROP        = hapus objek
TRUNCATE    = kosongkan tabel

NOT NULL    = wajib isi
UNIQUE      = unik
PRIMARY KEY = identitas unik row
FOREIGN KEY = hubungan antar tabel
CHECK       = validasi nilai

VIEW        = tabel virtual
SEQUENCE    = generator nomor
NEXTVAL     = nomor sequence berikutnya
CURRVAL     = nomor sequence terakhir
INDEX       = percepat pencarian
SYNONYM     = nama alternatif objek
```

---

# 34. Cara Belajar dengan File Ini

Untuk soal pilihan ganda:

1. Cari keyword yang muncul pada soal.
2. Baca definisinya.
3. Lihat perbedaan dengan istilah yang mirip.
4. Perhatikan contoh syntax agar tidak tertukar fungsi.
5. Gunakan bagian "Pola Soal Pilihan Ganda" untuk review cepat sebelum ujian.

Untuk soal studi kasus, gunakan `README.md` utama karena di sana query dan pola penyelesaian dibuat lebih lengkap.
