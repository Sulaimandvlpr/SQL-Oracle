# SQL Exam Query Handbook

Panduan ini disusun untuk kebutuhan ujian berbasis studi kasus menggunakan Oracle SQL. Fokus utama bukan sekadar definisi, tetapi pola berpikir, sintaks, dan query yang dapat langsung disalin lalu disesuaikan dengan kebutuhan soal.

> Gunakan `Ctrl + F` di GitHub untuk mencari kata seperti `rata-rata`, `lebih dari`, `nama diawali`, `gabungkan tabel`, `manager`, `subquery`, `insert`, `constraint`, `view`, `sequence`, dan sebagainya.

---

## 0. Cara Memakai Catatan Ini Saat Ujian

Urutan membaca soal:

1. Tentukan data apa yang harus ditampilkan.
2. Tentukan tabel sumbernya.
3. Tentukan apakah butuh satu tabel atau beberapa tabel.
4. Tentukan kondisi baris dengan `WHERE`.
5. Tentukan apakah butuh fungsi baris tunggal, fungsi grup, atau subquery.
6. Jika ada agregasi per kelompok, gunakan `GROUP BY`.
7. Jika filter dilakukan setelah agregasi, gunakan `HAVING`.
8. Jika hasil harus diurutkan, gunakan `ORDER BY`.
9. Jika data harus ditambah, diubah, atau dihapus, gunakan DML (`INSERT`, `UPDATE`, `DELETE`, `MERGE`).
10. Jika struktur database yang berubah, gunakan DDL (`CREATE`, `ALTER`, `DROP`, `TRUNCATE`, dan sebagainya).

Urutan umum penulisan query:

```sql
SELECT ...
FROM ...
JOIN ...
ON ...
WHERE ...
GROUP BY ...
HAVING ...
ORDER BY ...;
```

Urutan pemrosesan logis yang penting untuk dipahami:

```text
FROM / JOIN
WHERE
GROUP BY
HAVING
SELECT
ORDER BY
```

---

# 1. Database Schema yang Sering Dipakai

## 1.1 Employees / Departments Schema

Tabel utama:

### EMPLOYEES

```text
EMPLOYEE_ID      NUMBER(6)       Primary Key
FIRST_NAME       VARCHAR2(20)
LAST_NAME        VARCHAR2(25)    NOT NULL
EMAIL            VARCHAR2(25)    UNIQUE
PHONE_NUMBER     VARCHAR2(20)
HIRE_DATE        DATE            NOT NULL
JOB_ID           VARCHAR2(10)    Foreign Key -> JOBS.JOB_ID
SALARY           NUMBER(8,2)
COMMISSION_PCT   NUMBER(2,2)
MANAGER_ID       NUMBER(6)       Foreign Key -> EMPLOYEES.EMPLOYEE_ID
DEPARTMENT_ID    NUMBER(4)       Foreign Key -> DEPARTMENTS.DEPARTMENT_ID
BONUS            VARCHAR2(5)
```

### DEPARTMENTS

```text
DEPARTMENT_ID    NUMBER(4)       Primary Key
DEPARTMENT_NAME  VARCHAR2(30)    NOT NULL
MANAGER_ID       NUMBER(6)       Foreign Key -> EMPLOYEES.EMPLOYEE_ID
LOCATION_ID      NUMBER(4)       Foreign Key -> LOCATIONS.LOCATION_ID
```

### JOBS

```text
JOB_ID       VARCHAR2(10)    Primary Key
JOB_TITLE    VARCHAR2(35)    NOT NULL
MIN_SALARY   NUMBER(6)
MAX_SALARY   NUMBER(6)
```

### JOB_HISTORY

```text
EMPLOYEE_ID    NUMBER(6)
START_DATE     DATE
END_DATE       DATE
JOB_ID         VARCHAR2(10)
DEPARTMENT_ID  NUMBER(4)
```

### LOCATIONS

```text
LOCATION_ID     NUMBER(4)       Primary Key
STREET_ADDRESS  VARCHAR2(40)
POSTAL_CODE     VARCHAR2(12)
CITY            VARCHAR2(30)    NOT NULL
STATE_PROVINCE  VARCHAR2(25)
COUNTRY_ID      NUMBER(4)       Foreign Key -> COUNTRIES.COUNTRY_ID
```

### COUNTRIES

```text
COUNTRY_ID               NUMBER(4)      Primary Key
REGION_ID                NUMBER(3)      Foreign Key -> REGIONS.REGION_ID
COUNTRY_NAME             VARCHAR2(70)
COUNTRY_TRANSLATED_NAME  VARCHAR2(40)
LOCATION                 VARCHAR2(90)
CAPITAL                  VARCHAR2(50)
AREA                     NUMBER(16)
COASTLINE                NUMBER(8)
LOWEST_ELEVATION         NUMBER(6)
LOWEST_ELEV_NAME         VARCHAR2(70)
HIGHEST_ELEVATION        NUMBER(6)
HIGHEST_ELEV_NAME        VARCHAR2(50)
DATE_OF_INDEPENDENCE     VARCHAR2(30)
NATIONAL_HOLIDAY_NAME    VARCHAR2(200)
NATIONAL_HOLIDAY_DATE    VARCHAR2(30)
POPULATION               NUMBER(12)
POPULATION_GROWTH_RATE   VARCHAR2(10)
LIFE_EXPECT_AT_BIRTH     NUMBER(8,2)
MEDIAN_AGE               NUMBER(8,2)
AIRPORTS                 NUMBER(6)
CLIMATE                  VARCHAR2(1000)
FIPS_ID                  CHAR(2)
INTERNET_EXTENSION       VARCHAR2(3)
FLAG                     BLOB
CURRENCY_CODE            VARCHAR2(7)
```

### REGIONS

```text
REGION_ID    NUMBER(3)      Primary Key
REGION_NAME  VARCHAR2(35)
```

### CURRENCIES

```text
CURRENCY_CODE  VARCHAR2(7)    Primary Key
CURRENCY_NAME  VARCHAR2(40)
COMMENTS       VARCHAR2(150)
```

### LANGUAGES

```text
LANGUAGE_ID    NUMBER(4)      Primary Key
LANGUAGE_NAME  VARCHAR2(50)   NOT NULL
```

### SPOKEN_LANGUAGES

```text
COUNTRY_ID   NUMBER(4)
LANGUAGE_ID  NUMBER(4)
OFFICIAL     VARCHAR2(4)
COMMENTS     VARCHAR2(512)
```

### JOB_GRADES

```text
GRADE_LEVEL  VARCHAR2(3)
LOWEST_SAL   NUMBER
HIGHEST_SAL  NUMBER
```

Relasi yang paling sering dipakai:

```text
EMPLOYEES.DEPARTMENT_ID = DEPARTMENTS.DEPARTMENT_ID
EMPLOYEES.JOB_ID        = JOBS.JOB_ID
EMPLOYEES.MANAGER_ID    = EMPLOYEES.EMPLOYEE_ID
DEPARTMENTS.LOCATION_ID = LOCATIONS.LOCATION_ID
LOCATIONS.COUNTRY_ID    = COUNTRIES.COUNTRY_ID
COUNTRIES.REGION_ID     = REGIONS.REGION_ID
COUNTRIES.CURRENCY_CODE = CURRENCIES.CURRENCY_CODE
```

Contoh join berantai:

```sql
SELECT e.first_name,
       e.last_name,
       d.department_name,
       l.city,
       c.country_name,
       r.region_name
FROM employees e
JOIN departments d
  ON e.department_id = d.department_id
JOIN locations l
  ON d.location_id = l.location_id
JOIN countries c
  ON l.country_id = c.country_id
JOIN regions r
  ON c.region_id = r.region_id;
```

## 1.2 WF Countries Schema

Tabel utama:

```text
WF_COUNTRY
WF_WORLD_REGION
WF_CURRENCY
WF_LANGUAGE
WF_SPOKEN_LANGUAGES
```

Contoh pola join:

```sql
SELECT c.country_name,
       r.region_name
FROM wf_country c
JOIN wf_world_region r
  ON c.region_id = r.region_id;
```

Contoh negara dan bahasa:

```sql
SELECT c.country_name,
       l.language_name,
       sl.official
FROM wf_country c
JOIN wf_spoken_languages sl
  ON c.country_id = sl.country_id
JOIN wf_language l
  ON sl.language_id = l.language_id;
```

## 1.3 Global Fast Foods Schema

Entitas utama:

```text
ORDER
ORDER_LINE
FOOD_ITEM
CUSTOMER
STAFF
PROMOTIONAL_MENU
REGULAR_MENU
SHIFT_ASSIGNMENT
SHIFT
```

Contoh pola order dan customer:

```sql
SELECT o.order_number,
       o.order_date,
       o.order_total,
       c.first_name,
       c.last_name
FROM orders o
JOIN customers c
  ON o.customer_id = c.id;
```

Catatan: sesuaikan nama fisik tabel/kolom dengan Table Design yang dipakai di environment APEX karena beberapa diagram ERD menggunakan nama konseptual.

## 1.4 DJs on Demand Schema

Entitas utama:

```text
SONG_TYPE
SONG
PLAY_LIST_ITEM
TRACK_LISTING
CD
EVENT
CLIENT
JOB_ASSIGNMENT
PARTNER
THEME
PACKAGE
PRIVATE_HOME
PUBLIC_SPACE
```

Contoh pola client dan event:

```sql
SELECT c.first_name,
       c.last_name,
       e.name,
       e.event_date,
       e.cost
FROM clients c
JOIN events e
  ON c.client_number = e.client_number;
```

Catatan: sesuaikan nama kolom foreign key dengan Table Design di APEX.

---

# 2. Peta Cepat Bahasa Soal ke SQL

| Jika soal mengatakan | Gunakan |
|---|---|
| tampilkan semua kolom | `SELECT *` |
| tampilkan kolom tertentu | `SELECT kolom1, kolom2` |
| nilai unik/tidak duplikat | `DISTINCT` |
| nama kolom output diganti | alias `AS` |
| gabungkan teks | `||` atau `CONCAT()` |
| hanya baris tertentu | `WHERE` |
| lebih dari | `>` |
| minimal / sekurang-kurangnya | `>=` |
| kurang dari | `<` |
| maksimal / tidak lebih dari | `<=` |
| tidak sama dengan | `<>` atau `!=` |
| di antara dua nilai | `BETWEEN ... AND ...` |
| termasuk beberapa nilai | `IN (...)` |
| bukan beberapa nilai | `NOT IN (...)` |
| dimulai dengan | `LIKE 'A%'` |
| diakhiri dengan | `LIKE '%A'` |
| mengandung | `LIKE '%A%'` |
| tepat satu karakter wildcard | `_` |
| kosong/tidak diketahui | `IS NULL` |
| tidak kosong | `IS NOT NULL` |
| semua kondisi harus benar | `AND` |
| salah satu kondisi benar | `OR` |
| membalik kondisi | `NOT` |
| urut naik | `ORDER BY ... ASC` |
| urut turun | `ORDER BY ... DESC` |
| huruf besar | `UPPER()` |
| huruf kecil | `LOWER()` |
| kapital awal kata | `INITCAP()` |
| panjang teks | `LENGTH()` |
| ambil bagian teks | `SUBSTR()` |
| posisi karakter | `INSTR()` |
| ganti teks | `REPLACE()` |
| hapus spasi/karakter tepi | `TRIM()` |
| pembulatan | `ROUND()` |
| potong angka | `TRUNC()` |
| sisa pembagian | `MOD()` |
| tanggal saat ini | `SYSDATE` |
| selisih bulan | `MONTHS_BETWEEN()` |
| tambah bulan | `ADD_MONTHS()` |
| hari tertentu berikutnya | `NEXT_DAY()` |
| hari terakhir bulan | `LAST_DAY()` |
| ubah ke teks | `TO_CHAR()` |
| ubah ke angka | `TO_NUMBER()` |
| ubah ke tanggal | `TO_DATE()` |
| ganti NULL | `NVL()` |
| pilihan berdasarkan NULL/tidak NULL | `NVL2()` |
| NULL jika dua nilai sama | `NULLIF()` |
| nilai non-NULL pertama | `COALESCE()` |
| IF-THEN-ELSE | `CASE` atau `DECODE()` |
| gabungkan tabel | `JOIN` |
| join dengan kolom bernama sama | `NATURAL JOIN` / `USING` |
| kondisi join eksplisit | `JOIN ... ON` |
| semua baris tabel kiri | `LEFT OUTER JOIN` |
| semua baris tabel kanan | `RIGHT OUTER JOIN` |
| semua dari dua tabel | `FULL OUTER JOIN` |
| tabel join ke dirinya sendiri | `SELF JOIN` |
| total | `SUM()` |
| rata-rata | `AVG()` |
| jumlah baris | `COUNT()` |
| nilai paling kecil | `MIN()` |
| nilai paling besar | `MAX()` |
| per departemen/per kategori | `GROUP BY` |
| filter hasil grup | `HAVING` |
| bandingkan dengan hasil query lain | subquery |
| apakah baris terkait ada | `EXISTS` |
| apakah baris terkait tidak ada | `NOT EXISTS` |
| gabungkan hasil dua SELECT tanpa duplikat | `UNION` |
| gabungkan hasil dua SELECT dengan duplikat | `UNION ALL` |
| hanya data yang sama di dua hasil | `INTERSECT` |
| data query pertama yang tidak ada di kedua | `MINUS` |
| tambah baris | `INSERT` |
| ubah baris | `UPDATE` |
| hapus baris | `DELETE` |
| buat tabel | `CREATE TABLE` |
| ubah struktur tabel | `ALTER TABLE` |
| kosongkan seluruh isi tabel | `TRUNCATE TABLE` |
| hapus objek tabel | `DROP TABLE` |
| buat view | `CREATE VIEW` |
| nomor otomatis | `SEQUENCE` / `NEXTVAL` |
| percepat pencarian | `INDEX` |
| nama alternatif objek | `SYNONYM` |

---

# 3. Modul 01 — Oracle APEX

Fokus: menjalankan query SQL dasar pada lingkungan Oracle APEX.

Query paling dasar:

```sql
SELECT *
FROM employees;
```

Memilih kolom tertentu:

```sql
SELECT employee_id,
       first_name,
       last_name,
       salary
FROM employees;
```

Dengan kondisi:

```sql
SELECT first_name,
       last_name,
       salary
FROM employees
WHERE salary > 5000;
```

Template:

```sql
SELECT kolom1, kolom2
FROM nama_tabel
WHERE kondisi;
```

---

# 4. Modul 02 — Teknologi Relational Database

Konsep penting:

```text
Table   = kumpulan data
Row     = satu record
Column  = satu atribut
Primary Key = identitas unik baris
Foreign Key = penghubung ke tabel lain
```

Melihat struktur tabel di Oracle:

```sql
DESCRIBE employees;
```

atau:

```sql
DESC employees;
```

Contoh relasi employee ke department:

```sql
SELECT e.employee_id,
       e.last_name,
       d.department_name
FROM employees e
JOIN departments d
  ON e.department_id = d.department_id;
```

---

# 5. Modul 03 — Anatomi Pernyataan SQL

## SELECT dan FROM

```sql
SELECT first_name, last_name
FROM employees;
```

## Projection

Memilih kolom tertentu:

```sql
SELECT salary
FROM employees;
```

## Selection

Memilih baris tertentu:

```sql
SELECT salary
FROM employees
WHERE last_name = 'Smith';
```

## Ekspresi aritmetika

```sql
SELECT last_name,
       salary,
       salary * 12 AS annual_salary
FROM employees;
```

```sql
SELECT last_name,
       salary,
       salary + 1000 AS new_salary
FROM employees;
```

Operator:

```text
+  penjumlahan
-  pengurangan
*  perkalian
/  pembagian
```

Gunakan tanda kurung untuk mengontrol prioritas:

```sql
SELECT last_name,
       salary * 12 + 1000 AS result_1,
       salary * (12 + 1000) AS result_2
FROM employees;
```

---

# 6. Modul 04 — Kolom, Karakter, dan Baris

## Alias kolom

```sql
SELECT first_name AS nama_depan,
       salary AS gaji
FROM employees;
```

Alias dengan spasi:

```sql
SELECT first_name AS "Nama Depan",
       salary AS "Gaji Bulanan"
FROM employees;
```

## Concatenation

```sql
SELECT first_name || ' ' || last_name AS full_name
FROM employees;
```

Menggabungkan literal:

```sql
SELECT first_name || ' earns ' || salary AS employee_info
FROM employees;
```

## DISTINCT

```sql
SELECT DISTINCT department_id
FROM employees;
```

Lebih dari satu kolom:

```sql
SELECT DISTINCT department_id, job_id
FROM employees;
```

## Literal teks

```sql
SELECT first_name,
       'Employee' AS type
FROM employees;
```

---

# 7. Modul 05 — Membatasi Baris yang Dipilih

## WHERE

```sql
SELECT first_name,
       last_name,
       salary
FROM employees
WHERE salary > 5000;
```

Teks harus memakai tanda petik tunggal:

```sql
SELECT first_name, last_name
FROM employees
WHERE last_name = 'Taylor';
```

Tanggal juga dapat dibandingkan dengan literal tanggal. Untuk format yang aman, gunakan `TO_DATE`:

```sql
SELECT last_name, hire_date
FROM employees
WHERE hire_date > TO_DATE('01-01-2000', 'DD-MM-YYYY');
```

Angka tidak perlu tanda petik:

```sql
SELECT last_name, salary
FROM employees
WHERE salary >= 6000;
```

---

# 8. Modul 06 — Operator Perbandingan

## Operator dasar

```sql
WHERE salary = 5000
WHERE salary > 5000
WHERE salary >= 5000
WHERE salary < 5000
WHERE salary <= 5000
WHERE salary <> 5000
```

## BETWEEN

```sql
SELECT last_name, salary
FROM employees
WHERE salary BETWEEN 5000 AND 10000;
```

Setara secara logika dengan:

```sql
WHERE salary >= 5000
AND salary <= 10000
```

## IN

```sql
SELECT last_name, department_id
FROM employees
WHERE department_id IN (10, 20, 50, 80);
```

## NOT IN

```sql
SELECT last_name, department_id
FROM employees
WHERE department_id NOT IN (10, 20, 50);
```

Perhatian: `NOT IN` dapat bermasalah jika daftar hasil subquery mengandung `NULL`.

## LIKE

Dimulai dengan S:

```sql
SELECT last_name
FROM employees
WHERE last_name LIKE 'S%';
```

Diakhiri dengan s:

```sql
SELECT last_name
FROM employees
WHERE last_name LIKE '%s';
```

Mengandung `an`:

```sql
SELECT last_name
FROM employees
WHERE last_name LIKE '%an%';
```

Karakter kedua adalah `o`:

```sql
SELECT last_name
FROM employees
WHERE last_name LIKE '_o%';
```

Mencari underscore literal:

```sql
SELECT last_name, job_id
FROM employees
WHERE job_id LIKE '%\_R%' ESCAPE '\';
```

## NULL

```sql
SELECT last_name, commission_pct
FROM employees
WHERE commission_pct IS NULL;
```

```sql
SELECT last_name, commission_pct
FROM employees
WHERE commission_pct IS NOT NULL;
```

Jangan gunakan:

```sql
WHERE commission_pct = NULL
```

---

# 9. Modul 07 — Perbandingan Logika dan Aturan Presedensi

## AND

```sql
SELECT last_name, hire_date, job_id
FROM employees
WHERE hire_date > TO_DATE('01-01-1998', 'DD-MM-YYYY')
  AND job_id LIKE 'SA%';
```

## OR

```sql
SELECT last_name, department_id
FROM employees
WHERE department_id = 50
   OR department_id = 80;
```

## NOT

```sql
SELECT last_name, job_id
FROM employees
WHERE NOT job_id = 'IT_PROG';
```

Lebih sederhana:

```sql
WHERE job_id <> 'IT_PROG'
```

## Prioritas AND dan OR

`AND` dievaluasi sebelum `OR`.

```sql
SELECT last_name, department_id, first_name
FROM employees
WHERE department_id IN (50, 80)
  AND first_name LIKE 'C%'
   OR last_name LIKE '%s%';
```

Jika maksud soal berbeda, gunakan tanda kurung:

```sql
SELECT last_name, department_id, first_name
FROM employees
WHERE (department_id IN (50, 80)
       OR first_name LIKE 'C%')
  AND last_name LIKE '%s%';
```

Aturan aman saat ujian: jika ada kombinasi `AND` dan `OR`, gunakan tanda kurung agar logika eksplisit.

---

# 10. Modul 08 — Mengurutkan Baris

## ASC

```sql
SELECT last_name, salary
FROM employees
ORDER BY salary ASC;
```

`ASC` adalah default:

```sql
ORDER BY salary;
```

## DESC

```sql
SELECT last_name, salary
FROM employees
ORDER BY salary DESC;
```

## Beberapa kolom

```sql
SELECT department_id, last_name
FROM employees
ORDER BY department_id ASC,
         last_name ASC;
```

## Menggunakan alias

```sql
SELECT last_name,
       salary * 12 AS annual_salary
FROM employees
ORDER BY annual_salary DESC;
```

## Menggunakan posisi kolom

```sql
SELECT last_name, salary
FROM employees
ORDER BY 2 DESC;
```

Lebih aman dan mudah dibaca menggunakan nama kolom atau alias.

---

# 11. Modul 09 — Pengantar Fungsi

Dua kelompok utama:

```text
Single-row function: satu hasil untuk setiap baris.
Group function: satu hasil untuk sekumpulan baris.
```

Single-row:

```sql
SELECT last_name,
       UPPER(last_name),
       ROUND(salary)
FROM employees;
```

Group:

```sql
SELECT AVG(salary)
FROM employees;
```

Gabungan fungsi:

```sql
SELECT UPPER(last_name),
       ROUND(salary * 1.10)
FROM employees;
```

---

# 12. Modul 10 — Manipulasi Huruf dan Karakter

## UPPER

```sql
SELECT UPPER(last_name)
FROM employees;
```

Case-insensitive search:

```sql
SELECT first_name, last_name
FROM employees
WHERE UPPER(last_name) = UPPER('taylor');
```

## LOWER

```sql
SELECT LOWER(email)
FROM employees;
```

## INITCAP

```sql
SELECT INITCAP('oracle academy')
FROM dual;
```

## CONCAT

```sql
SELECT CONCAT(first_name, last_name)
FROM employees;
```

Biasanya lebih fleksibel menggunakan `||`:

```sql
SELECT first_name || ' ' || last_name AS full_name
FROM employees;
```

## SUBSTR

```sql
SELECT last_name,
       SUBSTR(last_name, 1, 3) AS first_three
FROM employees;
```

## LENGTH

```sql
SELECT last_name,
       LENGTH(last_name) AS name_length
FROM employees;
```

## INSTR

```sql
SELECT last_name,
       INSTR(last_name, 'a') AS position_of_a
FROM employees;
```

## LPAD

```sql
SELECT LPAD(salary, 10, '*')
FROM employees;
```

## RPAD

```sql
SELECT RPAD(last_name, 20, '.')
FROM employees;
```

## TRIM

```sql
SELECT TRIM('   Oracle   ')
FROM dual;
```

## REPLACE

```sql
SELECT REPLACE('JACK and JUE', 'J', 'BL')
FROM dual;
```

## Variabel substitusi

```sql
SELECT employee_id, last_name, salary
FROM employees
WHERE department_id = &department_number;
```

Teks:

```sql
SELECT employee_id, last_name
FROM employees
WHERE job_id = '&job_code';
```

---

# 13. Modul 11 — Fungsi Angka

## ROUND

```sql
SELECT ROUND(45.926, 2)
FROM dual;
```

Hasil konsep: pembulatan dua angka desimal.

```sql
SELECT last_name,
       salary,
       ROUND(salary / 7, 2) AS result
FROM employees;
```

## TRUNC

```sql
SELECT TRUNC(45.926, 2)
FROM dual;
```

## MOD

```sql
SELECT MOD(10, 3)
FROM dual;
```

Menentukan ganjil/genap:

```sql
SELECT employee_id,
       CASE
           WHEN MOD(employee_id, 2) = 0 THEN 'EVEN'
           ELSE 'ODD'
       END AS category
FROM employees;
```

---

# 14. Modul 12 — Fungsi Tanggal

## SYSDATE

```sql
SELECT SYSDATE
FROM dual;
```

## Aritmetika tanggal

Tanggal + hari:

```sql
SELECT SYSDATE + 7
FROM dual;
```

Selisih hari:

```sql
SELECT last_name,
       SYSDATE - hire_date AS days_worked
FROM employees;
```

## MONTHS_BETWEEN

```sql
SELECT last_name,
       MONTHS_BETWEEN(SYSDATE, hire_date) AS months_worked
FROM employees;
```

## ADD_MONTHS

```sql
SELECT hire_date,
       ADD_MONTHS(hire_date, 6) AS six_months_later
FROM employees;
```

## NEXT_DAY

```sql
SELECT NEXT_DAY(SYSDATE, 'MONDAY')
FROM dual;
```

## LAST_DAY

```sql
SELECT LAST_DAY(SYSDATE)
FROM dual;
```

## ROUND tanggal

```sql
SELECT ROUND(SYSDATE, 'MONTH')
FROM dual;
```

## TRUNC tanggal

```sql
SELECT TRUNC(SYSDATE, 'MONTH')
FROM dual;
```

---

# 15. Modul 13 — Fungsi Konversi

## TO_CHAR untuk tanggal

```sql
SELECT last_name,
       TO_CHAR(hire_date, 'DD-MM-YYYY') AS hire_date
FROM employees;
```

```sql
SELECT TO_CHAR(SYSDATE, 'DAY, DD MONTH YYYY')
FROM dual;
```

## TO_CHAR untuk angka

```sql
SELECT last_name,
       TO_CHAR(salary, '999,999.00') AS formatted_salary
FROM employees;
```

Dengan currency:

```sql
SELECT TO_CHAR(salary, '$99,999.00')
FROM employees;
```

## TO_DATE

```sql
SELECT TO_DATE('25-12-2026', 'DD-MM-YYYY')
FROM dual;
```

```sql
SELECT last_name, hire_date
FROM employees
WHERE hire_date >= TO_DATE('01-01-2000', 'DD-MM-YYYY');
```

## TO_NUMBER

```sql
SELECT TO_NUMBER('1500') + 500
FROM dual;
```

---

# 16. Modul 14 — Fungsi NULL

## NVL

```sql
SELECT last_name,
       salary,
       NVL(commission_pct, 0) AS commission
FROM employees;
```

Perhitungan aman dari NULL:

```sql
SELECT last_name,
       salary + salary * NVL(commission_pct, 0) AS total_pay
FROM employees;
```

## NVL2

```sql
SELECT last_name,
       NVL2(commission_pct,
            'Has Commission',
            'No Commission') AS commission_status
FROM employees;
```

## NULLIF

```sql
SELECT NULLIF(10, 10)
FROM dual;
```

Jika sama, hasil `NULL`.

## COALESCE

```sql
SELECT last_name,
       COALESCE(commission_pct, salary, 0) AS first_available_value
FROM employees;
```

## Nested function

```sql
SELECT last_name,
       UPPER(NVL(commission_pct, 0))
FROM employees;
```

Catatan: tipe data fungsi yang disarangkan harus kompatibel.

---

# 17. Modul 15 — Catatan Sumber

Berkas `EP 15.pdf` tidak terdapat pada kumpulan file yang diberikan. Materi urutan 5-1, 5-2, dan 5-3 tetap tersedia melalui EP 13, EP 14/17, dan EP 16/18. Bagian ini tidak diisi dengan materi rekaan.

Untuk kebutuhan ujian, materi yang berdekatan adalah:

```text
5-1 Fungsi Konversi
5-2 Fungsi NULL
5-3 Ekspresi Kondisional
```

Gunakan bagian Modul 13, 14, 16, 17, dan 18 pada catatan ini.

---

# 18. Modul 16 — Ekspresi Kondisional

## CASE sederhana

```sql
SELECT last_name,
       job_id,
       CASE job_id
           WHEN 'IT_PROG' THEN 'IT'
           WHEN 'SA_REP'  THEN 'SALES'
           ELSE 'OTHER'
       END AS job_category
FROM employees;
```

## Searched CASE

```sql
SELECT last_name,
       salary,
       CASE
           WHEN salary >= 10000 THEN 'HIGH'
           WHEN salary >= 5000  THEN 'MEDIUM'
           ELSE 'LOW'
       END AS salary_level
FROM employees;
```

## CASE untuk perhitungan

```sql
SELECT last_name,
       salary,
       CASE
           WHEN department_id = 80 THEN salary * 1.10
           WHEN department_id = 50 THEN salary * 1.05
           ELSE salary
       END AS new_salary
FROM employees;
```

## DECODE

```sql
SELECT last_name,
       job_id,
       DECODE(job_id,
              'IT_PROG', 'IT',
              'SA_REP',  'SALES',
              'OTHER') AS job_category
FROM employees;
```

Secara umum, `CASE` lebih fleksibel untuk kondisi rentang dan perbandingan kompleks.

---

# 19. Modul 17 — Penguatan Fungsi NULL

EP 17 mengulang topik 5-2 Fungsi NULL. Query penting:

```sql
SELECT last_name,
       NVL(commission_pct, 0) AS commission
FROM employees;
```

```sql
SELECT last_name,
       NVL2(commission_pct, 'YES', 'NO') AS has_commission
FROM employees;
```

```sql
SELECT COALESCE(commission_pct, salary, 0)
FROM employees;
```

```sql
SELECT NULLIF(salary, 5000)
FROM employees;
```

---

# 20. Modul 18 — Penguatan Ekspresi Kondisional

EP 18 mengulang topik 5-3 Ekspresi Kondisional.

Template CASE yang mudah disesuaikan:

```sql
SELECT kolom,
       CASE
           WHEN kondisi_1 THEN hasil_1
           WHEN kondisi_2 THEN hasil_2
           ELSE hasil_lain
       END AS alias
FROM tabel;
```

Template DECODE:

```sql
SELECT DECODE(kolom,
              nilai_1, hasil_1,
              nilai_2, hasil_2,
              hasil_default) AS alias
FROM tabel;
```

---

# 21. Modul 19 — Cross Join dan Natural Join

## CROSS JOIN

Menghasilkan semua kombinasi baris dua tabel.

```sql
SELECT e.last_name,
       d.department_name
FROM employees e
CROSS JOIN departments d;
```

Jika tabel A memiliki 10 baris dan tabel B 5 baris, hasil dapat menjadi 50 baris.

## NATURAL JOIN

Menggabungkan tabel berdasarkan kolom dengan nama dan tipe data yang sama.

```sql
SELECT first_name,
       last_name,
       job_id,
       job_title
FROM employees
NATURAL JOIN jobs
WHERE department_id > 80;
```

Perhatian: `NATURAL JOIN` dapat berbahaya jika dua tabel memiliki lebih dari satu nama kolom sama yang tidak dimaksudkan sebagai penghubung.

---

# 22. Modul 20 — Klausa Join: USING, ON, dan Tiga Tabel

## JOIN ... USING

```sql
SELECT employee_id,
       last_name,
       department_name
FROM employees
JOIN departments
USING (department_id);
```

Dengan `USING`, jangan awali kolom yang dipakai di `USING` dengan alias tabel pada bagian tertentu yang mengacu langsung ke kolom tersebut.

## JOIN ... ON

```sql
SELECT e.employee_id,
       e.last_name,
       d.department_name
FROM employees e
JOIN departments d
  ON e.department_id = d.department_id;
```

## Join tiga tabel

```sql
SELECT e.last_name,
       d.department_name,
       l.city
FROM employees e
JOIN departments d
  ON e.department_id = d.department_id
JOIN locations l
  ON d.location_id = l.location_id;
```

## Join dengan filter tambahan

```sql
SELECT e.last_name,
       d.department_name,
       e.salary
FROM employees e
JOIN departments d
  ON e.department_id = d.department_id
WHERE e.salary > 8000;
```

---

# 23. Modul 21 — Inner Join versus Outer Join

## INNER JOIN

Hanya baris yang memiliki pasangan.

```sql
SELECT e.last_name,
       d.department_name
FROM employees e
INNER JOIN departments d
  ON e.department_id = d.department_id;
```

## LEFT OUTER JOIN

Semua baris kiri tetap tampil.

```sql
SELECT e.last_name,
       d.department_name
FROM employees e
LEFT OUTER JOIN departments d
  ON e.department_id = d.department_id;
```

## RIGHT OUTER JOIN

```sql
SELECT e.last_name,
       d.department_name
FROM employees e
RIGHT OUTER JOIN departments d
  ON e.department_id = d.department_id;
```

## FULL OUTER JOIN

```sql
SELECT e.last_name,
       d.department_name
FROM employees e
FULL OUTER JOIN departments d
  ON e.department_id = d.department_id;
```

Kata kunci soal:

```text
"semua karyawan, termasuk yang belum punya departemen" -> LEFT JOIN dari employees
"semua departemen, termasuk yang belum punya karyawan" -> RIGHT JOIN jika employees di kiri
"semua data dari kedua sisi" -> FULL OUTER JOIN
```

---

# 24. Modul 22 — Self-Join dan Kueri Hierarkis

## Self Join

Menampilkan employee dan manager dari tabel yang sama:

```sql
SELECT e.last_name AS employee,
       m.last_name AS manager
FROM employees e
LEFT JOIN employees m
  ON e.manager_id = m.employee_id;
```

## Hierarchical Query

```sql
SELECT employee_id,
       last_name,
       manager_id,
       LEVEL
FROM employees
START WITH manager_id IS NULL
CONNECT BY PRIOR employee_id = manager_id;
```

Menampilkan hierarki dengan indentasi:

```sql
SELECT LPAD(' ', 2 * (LEVEL - 1)) || last_name AS hierarchy
FROM employees
START WITH manager_id IS NULL
CONNECT BY PRIOR employee_id = manager_id;
```

Mulai dari manager tertentu:

```sql
SELECT employee_id,
       last_name,
       manager_id,
       LEVEL
FROM employees
START WITH employee_id = 100
CONNECT BY PRIOR employee_id = manager_id;
```

---

# 25. Modul 23 — Oracle Equijoin dan Produk Kartesius

## Equijoin sintaks lama Oracle

```sql
SELECT e.last_name,
       d.department_name
FROM employees e,
     departments d
WHERE e.department_id = d.department_id;
```

ANSI join lebih jelas:

```sql
SELECT e.last_name,
       d.department_name
FROM employees e
JOIN departments d
  ON e.department_id = d.department_id;
```

## Cartesian Product

Terjadi jika kondisi join tidak ada:

```sql
SELECT e.last_name,
       d.department_name
FROM employees e,
     departments d;
```

Hindari kecuali soal memang meminta semua kombinasi.

---

# 26. Modul 24 — Oracle Nonequijoin dan Outer Join

## Nonequijoin

Digunakan ketika relasi bukan `=`.

Contoh menghubungkan salary ke job grade:

```sql
SELECT e.last_name,
       e.salary,
       j.grade_level
FROM employees e
JOIN job_grades j
  ON e.salary BETWEEN j.lowest_sal AND j.highest_sal;
```

Sintaks lama Oracle:

```sql
SELECT e.last_name,
       e.salary,
       j.grade_level
FROM employees e,
     job_grades j
WHERE e.salary BETWEEN j.lowest_sal AND j.highest_sal;
```

## Outer Join lama Oracle `(+)`

```sql
SELECT e.last_name,
       d.department_name
FROM employees e,
     departments d
WHERE e.department_id = d.department_id(+);
```

Untuk ujian modern, prioritaskan ANSI `LEFT OUTER JOIN`, kecuali soal secara eksplisit meminta sintaks proprietary Oracle.

---

# 27. Modul 25 — Fungsi Grup

## SUM

```sql
SELECT SUM(salary) AS total_salary
FROM employees;
```

## AVG

```sql
SELECT AVG(salary) AS avg_salary
FROM employees;
```

## COUNT

```sql
SELECT COUNT(*) AS total_employees
FROM employees;
```

## MIN

```sql
SELECT MIN(salary) AS lowest_salary
FROM employees;
```

## MAX

```sql
SELECT MAX(salary) AS highest_salary
FROM employees;
```

## STDDEV

```sql
SELECT STDDEV(salary) AS salary_stddev
FROM employees;
```

## VARIANCE

```sql
SELECT VARIANCE(salary) AS salary_variance
FROM employees;
```

Beberapa fungsi sekaligus:

```sql
SELECT COUNT(*) AS total_employee,
       ROUND(AVG(salary), 2) AS avg_salary,
       MIN(salary) AS min_salary,
       MAX(salary) AS max_salary,
       SUM(salary) AS total_salary
FROM employees;
```

---

# 28. Modul 26 — COUNT, DISTINCT, dan NVL

## COUNT(*)

Menghitung semua baris:

```sql
SELECT COUNT(*)
FROM employees;
```

## COUNT(column)

Mengabaikan NULL:

```sql
SELECT COUNT(commission_pct)
FROM employees;
```

## COUNT(DISTINCT ...)

```sql
SELECT COUNT(DISTINCT department_id)
FROM employees;
```

## AVG dengan NVL

Jika soal meminta NULL dianggap nol:

```sql
SELECT AVG(NVL(commission_pct, 0))
FROM employees;
```

Penting: ini tidak sama dengan `AVG(commission_pct)` karena `AVG(column)` normalnya mengabaikan NULL.

---

# 29. Modul 27 — Penguatan Fungsi Grup

EP 27 mengulang topik Fungsi Grup.

Template paling sering:

```sql
SELECT SUM(kolom),
       AVG(kolom),
       MIN(kolom),
       MAX(kolom),
       COUNT(*)
FROM tabel;
```

Dengan kondisi:

```sql
SELECT AVG(salary)
FROM employees
WHERE department_id = 80;
```

---

# 30. Modul 28 — Penguatan COUNT, DISTINCT, NVL

EP 28 mengulang topik COUNT, DISTINCT, dan NVL.

```sql
SELECT COUNT(*)
FROM employees;
```

```sql
SELECT COUNT(DISTINCT job_id)
FROM employees;
```

```sql
SELECT AVG(NVL(commission_pct, 0))
FROM employees;
```

---

# 31. Modul 29 — GROUP BY dan HAVING

## GROUP BY

Rata-rata gaji per departemen:

```sql
SELECT department_id,
       AVG(salary) AS avg_salary
FROM employees
GROUP BY department_id;
```

Jumlah employee per department:

```sql
SELECT department_id,
       COUNT(*) AS employee_count
FROM employees
GROUP BY department_id;
```

## GROUP BY beberapa kolom

```sql
SELECT department_id,
       job_id,
       AVG(salary) AS avg_salary
FROM employees
GROUP BY department_id, job_id;
```

## HAVING

Department dengan rata-rata gaji > 8000:

```sql
SELECT department_id,
       AVG(salary) AS avg_salary
FROM employees
GROUP BY department_id
HAVING AVG(salary) > 8000;
```

## WHERE dan HAVING bersama

```sql
SELECT department_id,
       AVG(salary) AS avg_salary
FROM employees
WHERE job_id <> 'AD_PRES'
GROUP BY department_id
HAVING AVG(salary) > 5000
ORDER BY avg_salary DESC;
```

Ingat:

```text
WHERE  -> filter baris sebelum dikelompokkan
HAVING -> filter kelompok setelah GROUP BY
```

## Nested group function

```sql
SELECT MAX(AVG(salary))
FROM employees
GROUP BY department_id;
```

---

# 32. Modul 30 — ROLLUP, CUBE, GROUPING SETS

## ROLLUP

Menghasilkan subtotal hierarkis dan grand total:

```sql
SELECT department_id,
       job_id,
       SUM(salary) AS total_salary
FROM employees
GROUP BY ROLLUP(department_id, job_id);
```

## CUBE

Menghasilkan kombinasi subtotal lebih lengkap:

```sql
SELECT department_id,
       job_id,
       SUM(salary) AS total_salary
FROM employees
GROUP BY CUBE(department_id, job_id);
```

## GROUPING SETS

```sql
SELECT department_id,
       job_id,
       SUM(salary) AS total_salary
FROM employees
GROUP BY GROUPING SETS (
    (department_id),
    (job_id),
    ()
);
```

## GROUPING

Membedakan NULL asli dan NULL hasil subtotal:

```sql
SELECT department_id,
       GROUPING(department_id) AS is_summary,
       SUM(salary) AS total_salary
FROM employees
GROUP BY ROLLUP(department_id);
```

---

# 33. Modul 31 — Operator Set

Syarat penting:

```text
Jumlah kolom pada kedua SELECT harus sama.
Tipe data kolom yang bersesuaian harus kompatibel.
ORDER BY diletakkan di akhir seluruh set operation.
```

## UNION

Menggabungkan dan menghapus duplikat:

```sql
SELECT employee_id
FROM employees
WHERE department_id = 50
UNION
SELECT employee_id
FROM job_history
WHERE department_id = 50;
```

## UNION ALL

Menjaga duplikat:

```sql
SELECT employee_id
FROM employees
UNION ALL
SELECT employee_id
FROM job_history;
```

## INTERSECT

```sql
SELECT employee_id
FROM employees
INTERSECT
SELECT employee_id
FROM job_history;
```

## MINUS

```sql
SELECT employee_id
FROM employees
MINUS
SELECT employee_id
FROM job_history;
```

## Dengan ORDER BY

```sql
SELECT employee_id, job_id
FROM employees
UNION
SELECT employee_id, job_id
FROM job_history
ORDER BY employee_id;
```

---

# 34. Modul 32 — Dasar-Dasar Subquery

Pola umum:

```sql
SELECT ...
FROM ...
WHERE kolom operator (
    SELECT ...
    FROM ...
    WHERE ...
);
```

Contoh: employee dengan gaji lebih tinggi dari Abel:

```sql
SELECT last_name, salary
FROM employees
WHERE salary > (
    SELECT salary
    FROM employees
    WHERE last_name = 'Abel'
);
```

Contoh berdasarkan department employee tertentu:

```sql
SELECT last_name, department_id
FROM employees
WHERE department_id = (
    SELECT department_id
    FROM employees
    WHERE employee_id = 101
);
```

Aturan penting: kerjakan subquery secara mental lebih dulu, lalu tentukan operator query utama.

---

# 35. Modul 33 — Subquery Baris Tunggal

Single-row subquery mengembalikan maksimal satu baris.

Operator yang cocok:

```text
=
>
>=
<
<=
<>
```

## Sama dengan rata-rata tertentu

```sql
SELECT last_name, salary
FROM employees
WHERE salary > (
    SELECT AVG(salary)
    FROM employees
);
```

## Gaji sama dengan maksimum

```sql
SELECT last_name, salary
FROM employees
WHERE salary = (
    SELECT MAX(salary)
    FROM employees
);
```

## Subquery dalam HAVING

```sql
SELECT department_id,
       AVG(salary) AS avg_salary
FROM employees
GROUP BY department_id
HAVING AVG(salary) > (
    SELECT AVG(salary)
    FROM employees
);
```

Jika subquery dapat menghasilkan banyak baris, jangan gunakan `=`; gunakan operator multi-row yang sesuai.

---

# 36. Modul 34 — Subquery Multi-Baris

Operator utama:

```text
IN
ANY
ALL
```

## IN

```sql
SELECT last_name, department_id
FROM employees
WHERE department_id IN (
    SELECT department_id
    FROM departments
    WHERE location_id = 1700
);
```

## > ANY

Lebih besar dari setidaknya satu nilai:

```sql
SELECT last_name, salary
FROM employees
WHERE salary > ANY (
    SELECT salary
    FROM employees
    WHERE department_id = 50
);
```

## > ALL

Lebih besar dari semua nilai:

```sql
SELECT last_name, salary
FROM employees
WHERE salary > ALL (
    SELECT salary
    FROM employees
    WHERE department_id = 50
);
```

## < ANY

Lebih kecil dari setidaknya satu nilai.

```sql
SELECT last_name, salary
FROM employees
WHERE salary < ANY (
    SELECT salary
    FROM employees
    WHERE department_id = 80
);
```

## < ALL

Lebih kecil dari semua nilai.

```sql
SELECT last_name, salary
FROM employees
WHERE salary < ALL (
    SELECT salary
    FROM employees
    WHERE department_id = 80
);
```

## Pair-wise multi-column subquery

```sql
SELECT employee_id,
       department_id,
       manager_id
FROM employees
WHERE (manager_id, department_id) IN (
    SELECT manager_id, department_id
    FROM employees
    WHERE employee_id IN (174, 199)
);
```

---

# 37. Modul 35 — Correlated Subquery, EXISTS, NOT EXISTS, WITH

## Correlated subquery

Employee yang gajinya lebih tinggi dari rata-rata departemennya:

```sql
SELECT e.employee_id,
       e.last_name,
       e.salary,
       e.department_id
FROM employees e
WHERE e.salary > (
    SELECT AVG(e2.salary)
    FROM employees e2
    WHERE e2.department_id = e.department_id
);
```

Subquery mengacu pada baris query luar.

## EXISTS

Department yang memiliki employee:

```sql
SELECT d.department_id,
       d.department_name
FROM departments d
WHERE EXISTS (
    SELECT 1
    FROM employees e
    WHERE e.department_id = d.department_id
);
```

## NOT EXISTS

Department tanpa employee:

```sql
SELECT d.department_id,
       d.department_name
FROM departments d
WHERE NOT EXISTS (
    SELECT 1
    FROM employees e
    WHERE e.department_id = d.department_id
);
```

## WITH clause

```sql
WITH dept_avg AS (
    SELECT department_id,
           AVG(salary) AS avg_salary
    FROM employees
    GROUP BY department_id
)
SELECT e.last_name,
       e.salary,
       e.department_id,
       d.avg_salary
FROM employees e
JOIN dept_avg d
  ON e.department_id = d.department_id
WHERE e.salary > d.avg_salary;
```

---

# 38. Modul 36 — Memastikan Kualitas Hasil Query

Query yang valid secara sintaks belum tentu benar secara logika.

Checklist sebelum final:

```text
1. Kolom output sesuai soal?
2. Tabel sumber benar?
3. Join condition lengkap?
4. Ada Cartesian product tidak sengaja?
5. WHERE terlalu luas/sempit?
6. AND/OR sudah diberi tanda kurung?
7. NULL ditangani dengan IS NULL / NVL bila perlu?
8. Aggregate dan GROUP BY konsisten?
9. HAVING dipakai untuk kondisi aggregate?
10. Subquery mengembalikan satu atau banyak baris?
11. Duplikat perlu dipertahankan atau dihapus?
12. ORDER BY sesuai permintaan?
```

Contoh cek query salah:

```sql
SELECT e.last_name,
       d.department_name
FROM employees e,
     departments d;
```

Masalah: tidak ada kondisi join sehingga menghasilkan Cartesian product.

Perbaikan:

```sql
SELECT e.last_name,
       d.department_name
FROM employees e
JOIN departments d
  ON e.department_id = d.department_id;
```

---

# 39. Modul 37 — INSERT

## INSERT semua kolom

```sql
INSERT INTO departments
VALUES (280, 'Data Science', 100, 1700);
```

Gunakan hanya jika urutan dan jumlah nilai cocok persis dengan struktur tabel.

## INSERT kolom tertentu

```sql
INSERT INTO departments
       (department_id, department_name, location_id)
VALUES (280, 'Data Science', 1700);
```

## INSERT dengan NULL eksplisit

```sql
INSERT INTO departments
       (department_id, department_name, manager_id, location_id)
VALUES (281, 'Research', NULL, 1700);
```

## INSERT tanggal

```sql
INSERT INTO employees
       (employee_id, first_name, last_name, email,
        hire_date, job_id, salary)
VALUES (300, 'Alya', 'Putri', 'APUTRI',
        TO_DATE('05-10-2026', 'DD-MM-YYYY'),
        'IT_PROG', 6000);
```

## INSERT dari subquery

```sql
INSERT INTO sales_reps
       (employee_id, last_name, salary)
SELECT employee_id, last_name, salary
FROM employees
WHERE job_id = 'SA_REP';
```

---

# 40. Modul 38 — UPDATE dan DELETE

## UPDATE satu kolom

```sql
UPDATE employees
SET salary = 7000
WHERE employee_id = 200;
```

## UPDATE beberapa kolom

```sql
UPDATE employees
SET salary = 7500,
    department_id = 80
WHERE employee_id = 200;
```

## UPDATE berdasarkan nilai lama

```sql
UPDATE employees
SET salary = salary * 1.10
WHERE department_id = 80;
```

## UPDATE dengan subquery

```sql
UPDATE employees
SET salary = (
    SELECT AVG(salary)
    FROM employees
)
WHERE employee_id = 200;
```

## DELETE baris tertentu

```sql
DELETE FROM employees
WHERE employee_id = 300;
```

## DELETE berdasarkan subquery

```sql
DELETE FROM employees
WHERE department_id IN (
    SELECT department_id
    FROM departments
    WHERE location_id = 1800
);
```

## Bahaya WHERE

Ini mengubah semua baris:

```sql
UPDATE employees
SET salary = 5000;
```

Ini menghapus semua baris:

```sql
DELETE FROM employees;
```

## Transaction Control

Simpan permanen:

```sql
COMMIT;
```

Batalkan perubahan yang belum di-commit:

```sql
ROLLBACK;
```

Savepoint:

```sql
SAVEPOINT before_update;

UPDATE employees
SET salary = salary * 1.10
WHERE department_id = 80;

ROLLBACK TO before_update;
```

---

# 41. Modul 39 — DEFAULT, MERGE, dan INSERT Multi-Tabel

## DEFAULT pada INSERT

Jika kolom memiliki default:

```sql
INSERT INTO nama_tabel (id, status)
VALUES (1, DEFAULT);
```

## MERGE

Pola umum:

```sql
MERGE INTO target_table t
USING source_table s
ON (t.id = s.id)
WHEN MATCHED THEN
    UPDATE SET t.nama = s.nama
WHEN NOT MATCHED THEN
    INSERT (id, nama)
    VALUES (s.id, s.nama);
```

Contoh employees staging:

```sql
MERGE INTO employees e
USING new_employees n
ON (e.employee_id = n.employee_id)
WHEN MATCHED THEN
    UPDATE SET e.salary = n.salary
WHEN NOT MATCHED THEN
    INSERT (employee_id, last_name, email, hire_date, job_id, salary)
    VALUES (n.employee_id, n.last_name, n.email,
            n.hire_date, n.job_id, n.salary);
```

## INSERT ALL

```sql
INSERT ALL
    INTO table_a (id, name) VALUES (id, name)
    INTO table_b (id, name) VALUES (id, name)
SELECT employee_id AS id,
       last_name AS name
FROM employees
WHERE department_id = 80;
```

## Conditional multi-table insert

```sql
INSERT ALL
    WHEN salary < 5000 THEN
        INTO low_salary (employee_id, salary)
        VALUES (employee_id, salary)
    WHEN salary >= 5000 THEN
        INTO high_salary (employee_id, salary)
        VALUES (employee_id, salary)
SELECT employee_id, salary
FROM employees;
```

---

# 42. Modul 40 — Membuat Tabel

## CREATE TABLE dasar

```sql
CREATE TABLE students (
    student_id NUMBER(6),
    first_name VARCHAR2(30),
    last_name  VARCHAR2(30),
    birth_date DATE,
    gpa        NUMBER(3,2)
);
```

## Dengan constraint sederhana

```sql
CREATE TABLE students (
    student_id NUMBER(6) PRIMARY KEY,
    full_name  VARCHAR2(100) NOT NULL,
    email      VARCHAR2(100) UNIQUE,
    gpa        NUMBER(3,2)
);
```

## CREATE TABLE AS SELECT (CTAS)

```sql
CREATE TABLE emp_copy AS
SELECT employee_id,
       first_name,
       last_name,
       salary,
       department_id
FROM employees;
```

Hanya struktur tanpa data:

```sql
CREATE TABLE emp_structure AS
SELECT employee_id,
       first_name,
       last_name,
       salary
FROM employees
WHERE 1 = 2;
```

## Data Dictionary

Daftar tabel milik user:

```sql
SELECT table_name
FROM user_tables;
```

Daftar objek:

```sql
SELECT object_name,
       object_type,
       status
FROM user_objects;
```

---

# 43. Modul 41 — Menggunakan Tipe Data

Tipe data yang sering dipakai:

```text
VARCHAR2(n)        teks panjang bervariasi
CHAR(n)            teks panjang tetap
NUMBER             angka
NUMBER(p,s)        angka dengan precision dan scale
DATE               tanggal + waktu sampai detik
TIMESTAMP          tanggal-waktu dengan fractional seconds
TIMESTAMP WITH TIME ZONE
INTERVAL YEAR TO MONTH
INTERVAL DAY TO SECOND
CLOB               teks sangat besar
BLOB               data biner besar
```

## Contoh VARCHAR2, NUMBER, DATE

```sql
CREATE TABLE products (
    product_id   NUMBER(6),
    product_name VARCHAR2(100),
    price        NUMBER(10,2),
    created_date DATE
);
```

## TIMESTAMP

```sql
CREATE TABLE logs (
    log_id      NUMBER,
    event_time  TIMESTAMP,
    description VARCHAR2(200)
);
```

Insert timestamp:

```sql
INSERT INTO logs
VALUES (1, SYSTIMESTAMP, 'Login');
```

## TIMESTAMP WITH TIME ZONE

```sql
CREATE TABLE meetings (
    meeting_id   NUMBER,
    meeting_time TIMESTAMP WITH TIME ZONE
);
```

```sql
INSERT INTO meetings
VALUES (
    1,
    TO_TIMESTAMP_TZ(
        '05-10-2026 09:00:00 +07:00',
        'DD-MM-YYYY HH24:MI:SS TZH:TZM'
    )
);
```

## INTERVAL YEAR TO MONTH

```sql
CREATE TABLE contracts (
    contract_id NUMBER,
    duration    INTERVAL YEAR TO MONTH
);
```

```sql
INSERT INTO contracts
VALUES (1, INTERVAL '2-6' YEAR TO MONTH);
```

## INTERVAL DAY TO SECOND

```sql
CREATE TABLE process_log (
    process_id NUMBER,
    duration   INTERVAL DAY TO SECOND
);
```

```sql
INSERT INTO process_log
VALUES (1, INTERVAL '2 03:30:00' DAY TO SECOND);
```

---

# 44. Modul 42 — Memodifikasi Tabel

## ADD column

```sql
ALTER TABLE employees
ADD middle_name VARCHAR2(30);
```

Beberapa kolom:

```sql
ALTER TABLE employees
ADD (
    nickname VARCHAR2(20),
    status   VARCHAR2(10)
);
```

## MODIFY column

```sql
ALTER TABLE employees
MODIFY middle_name VARCHAR2(50);
```

## DROP column

```sql
ALTER TABLE employees
DROP COLUMN middle_name;
```

## RENAME column

```sql
ALTER TABLE employees
RENAME COLUMN nickname TO preferred_name;
```

## RENAME table

```sql
RENAME old_table TO new_table;
```

## TRUNCATE

Menghapus seluruh baris dengan DDL:

```sql
TRUNCATE TABLE emp_copy;
```

## DROP TABLE

```sql
DROP TABLE emp_copy;
```

## FLASHBACK TABLE

Contoh pola:

```sql
FLASHBACK TABLE employees
TO TIMESTAMP (SYSTIMESTAMP - INTERVAL '5' MINUTE);
```

Penggunaan flashback membutuhkan konfigurasi/hak akses yang sesuai.

---

# 45. Modul 43 — Constraint NOT NULL dan UNIQUE

## NOT NULL level kolom

```sql
CREATE TABLE students (
    student_id NUMBER,
    full_name  VARCHAR2(100) NOT NULL
);
```

Dengan nama constraint:

```sql
CREATE TABLE students (
    student_id NUMBER,
    full_name  VARCHAR2(100)
        CONSTRAINT students_name_nn NOT NULL
);
```

## UNIQUE level kolom

```sql
CREATE TABLE students (
    student_id NUMBER,
    email      VARCHAR2(100)
        CONSTRAINT students_email_uk UNIQUE
);
```

## UNIQUE level tabel

```sql
CREATE TABLE students (
    student_id NUMBER,
    email      VARCHAR2(100),
    CONSTRAINT students_email_uk UNIQUE (email)
);
```

## Composite UNIQUE

```sql
CREATE TABLE registrations (
    student_id NUMBER,
    course_id  NUMBER,
    semester   VARCHAR2(20),
    CONSTRAINT registrations_uk
        UNIQUE (student_id, course_id, semester)
);
```

---

# 46. Modul 44 — PRIMARY KEY, FOREIGN KEY, CHECK

## PRIMARY KEY level kolom

```sql
CREATE TABLE departments_test (
    department_id NUMBER
        CONSTRAINT dept_test_pk PRIMARY KEY,
    department_name VARCHAR2(50)
);
```

## PRIMARY KEY level tabel

```sql
CREATE TABLE departments_test (
    department_id NUMBER,
    department_name VARCHAR2(50),
    CONSTRAINT dept_test_pk PRIMARY KEY (department_id)
);
```

## Composite PRIMARY KEY

Harus didefinisikan pada level tabel:

```sql
CREATE TABLE enrollment (
    student_id NUMBER,
    course_id  NUMBER,
    CONSTRAINT enrollment_pk
        PRIMARY KEY (student_id, course_id)
);
```

## FOREIGN KEY

```sql
CREATE TABLE students (
    student_id    NUMBER PRIMARY KEY,
    department_id NUMBER,
    CONSTRAINT students_dept_fk
        FOREIGN KEY (department_id)
        REFERENCES departments(department_id)
);
```

## FOREIGN KEY dengan ON DELETE CASCADE

```sql
CREATE TABLE child_table (
    child_id  NUMBER PRIMARY KEY,
    parent_id NUMBER,
    CONSTRAINT child_parent_fk
        FOREIGN KEY (parent_id)
        REFERENCES parent_table(parent_id)
        ON DELETE CASCADE
);
```

## CHECK

```sql
CREATE TABLE employees_test (
    employee_id NUMBER PRIMARY KEY,
    salary      NUMBER(8,2),
    CONSTRAINT employees_salary_ck
        CHECK (salary > 0)
);
```

```sql
CREATE TABLE students (
    student_id NUMBER PRIMARY KEY,
    gender     CHAR(1),
    CONSTRAINT students_gender_ck
        CHECK (gender IN ('M', 'F'))
);
```

---

# 47. Modul 45 — Mengelola Constraint

## Menambah PRIMARY KEY

```sql
ALTER TABLE students
ADD CONSTRAINT students_pk
PRIMARY KEY (student_id);
```

## Menambah FOREIGN KEY

```sql
ALTER TABLE students
ADD CONSTRAINT students_dept_fk
FOREIGN KEY (department_id)
REFERENCES departments(department_id);
```

## Menambah CHECK

```sql
ALTER TABLE students
ADD CONSTRAINT students_gpa_ck
CHECK (gpa BETWEEN 0 AND 4);
```

## Menghapus constraint

```sql
ALTER TABLE students
DROP CONSTRAINT students_gpa_ck;
```

## DISABLE

```sql
ALTER TABLE students
DISABLE CONSTRAINT students_dept_fk;
```

## ENABLE

```sql
ALTER TABLE students
ENABLE CONSTRAINT students_dept_fk;
```

## DROP PRIMARY KEY

```sql
ALTER TABLE students
DROP PRIMARY KEY;
```

## CASCADE

```sql
ALTER TABLE parent_table
DROP PRIMARY KEY CASCADE;
```

## Data Dictionary constraint

```sql
SELECT constraint_name,
       constraint_type,
       table_name,
       status
FROM user_constraints
WHERE table_name = 'STUDENTS';
```

Kolom constraint:

```sql
SELECT constraint_name,
       table_name,
       column_name
FROM user_cons_columns
WHERE table_name = 'STUDENTS';
```

Kode tipe umum:

```text
P = Primary Key
R = Foreign Key
U = Unique
C = Check / Not Null
```

---

# 48. Modul 46 — Membuat View

## Simple View

```sql
CREATE VIEW emp_view AS
SELECT employee_id,
       first_name,
       last_name,
       salary
FROM employees;
```

Mengambil data:

```sql
SELECT *
FROM emp_view;
```

## View dengan alias kolom

```sql
CREATE VIEW emp_salary_view
       (id, employee_name, salary) AS
SELECT employee_id,
       first_name || ' ' || last_name,
       salary
FROM employees;
```

## CREATE OR REPLACE VIEW

```sql
CREATE OR REPLACE VIEW emp_view AS
SELECT employee_id,
       last_name,
       salary,
       department_id
FROM employees
WHERE department_id = 80;
```

## Complex View

```sql
CREATE VIEW dept_salary_view AS
SELECT d.department_name,
       COUNT(e.employee_id) AS employee_count,
       AVG(e.salary) AS avg_salary
FROM departments d
JOIN employees e
  ON d.department_id = e.department_id
GROUP BY d.department_name;
```

## WITH READ ONLY

```sql
CREATE OR REPLACE VIEW emp_readonly AS
SELECT employee_id,
       last_name,
       salary
FROM employees
WITH READ ONLY;
```

---

# 49. Modul 47 — Operasi DML dan View

DML pada simple view dapat dilakukan jika view memenuhi syarat tertentu.

## UPDATE melalui view

```sql
UPDATE emp_view
SET salary = 7000
WHERE employee_id = 200;
```

## INSERT melalui view

```sql
INSERT INTO emp_view
       (employee_id, first_name, last_name, salary)
VALUES (300, 'Alya', 'Putri', 6000);
```

Keberhasilan bergantung pada struktur view dan constraint base table.

## WITH CHECK OPTION

Mencegah DML melalui view yang membuat baris keluar dari kondisi view.

```sql
CREATE OR REPLACE VIEW dept80_view AS
SELECT employee_id,
       last_name,
       department_id,
       salary
FROM employees
WHERE department_id = 80
WITH CHECK OPTION;
```

Update berikut akan ditolak karena hasil baris tidak lagi memenuhi `department_id = 80`:

```sql
UPDATE dept80_view
SET department_id = 50
WHERE employee_id = 145;
```

## WITH READ ONLY

```sql
CREATE OR REPLACE VIEW dept80_readonly AS
SELECT employee_id,
       last_name,
       department_id
FROM employees
WHERE department_id = 80
WITH READ ONLY;
```

---

# 50. Modul 48 — Mengelola View, Inline View, Top-N Analysis

## DROP VIEW

```sql
DROP VIEW emp_view;
```

## Inline View

Subquery pada `FROM`:

```sql
SELECT department_id,
       avg_salary
FROM (
    SELECT department_id,
           AVG(salary) AS avg_salary
    FROM employees
    GROUP BY department_id
)
WHERE avg_salary > 8000;
```

## Top-N dengan ROWNUM

Lima employee dengan gaji tertinggi:

```sql
SELECT *
FROM (
    SELECT employee_id,
           last_name,
           salary
    FROM employees
    ORDER BY salary DESC
)
WHERE ROWNUM <= 5;
```

Penting: pengurutan harus dilakukan di subquery sebelum `ROWNUM` dibatasi.

## Rentang ranking dengan inline view

```sql
SELECT employee_id,
       last_name,
       salary
FROM (
    SELECT e.*,
           ROWNUM AS rn
    FROM (
        SELECT employee_id,
               last_name,
               salary
        FROM employees
        ORDER BY salary DESC
    ) e
    WHERE ROWNUM <= 10
)
WHERE rn >= 6;
```

---

# 51. Modul 49 — Sequence

## CREATE SEQUENCE

```sql
CREATE SEQUENCE employee_seq
START WITH 300
INCREMENT BY 1
MAXVALUE 999999
NOCYCLE
CACHE 20;
```

Versi sederhana:

```sql
CREATE SEQUENCE employee_seq
START WITH 300
INCREMENT BY 1;
```

## NEXTVAL

Menghasilkan nilai berikutnya:

```sql
SELECT employee_seq.NEXTVAL
FROM dual;
```

Menggunakan sequence saat INSERT:

```sql
INSERT INTO employees
       (employee_id, first_name, last_name, email,
        hire_date, job_id, salary)
VALUES (employee_seq.NEXTVAL,
        'Alya',
        'Putri',
        'APUTRI',
        SYSDATE,
        'IT_PROG',
        6000);
```

## CURRVAL

```sql
SELECT employee_seq.CURRVAL
FROM dual;
```

`CURRVAL` baru dapat digunakan dalam sesi setelah `NEXTVAL` pernah dipanggil.

## USER_SEQUENCES

```sql
SELECT sequence_name,
       min_value,
       max_value,
       increment_by,
       last_number
FROM user_sequences;
```

## ALTER SEQUENCE

```sql
ALTER SEQUENCE employee_seq
INCREMENT BY 10
MAXVALUE 9999999;
```

## DROP SEQUENCE

```sql
DROP SEQUENCE employee_seq;
```

---

# 52. Modul 50 — Index dan Synonym

## CREATE INDEX

```sql
CREATE INDEX emp_last_name_idx
ON employees(last_name);
```

## Composite index

```sql
CREATE INDEX emp_dept_job_idx
ON employees(department_id, job_id);
```

## Function-based index

```sql
CREATE INDEX emp_upper_last_name_idx
ON employees(UPPER(last_name));
```

Query yang dapat memanfaatkan pola tersebut:

```sql
SELECT employee_id,
       last_name
FROM employees
WHERE UPPER(last_name) = 'TAYLOR';
```

## DROP INDEX

```sql
DROP INDEX emp_last_name_idx;
```

## Private Synonym

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

Membutuhkan privilege yang sesuai:

```sql
CREATE PUBLIC SYNONYM emp_public
FOR hr.employees;
```

## DROP SYNONYM

```sql
DROP SYNONYM emp;
```

```sql
DROP PUBLIC SYNONYM emp_public;
```

---

# 53. Template Query Studi Kasus Siap Salin

## 53.1 Cari employee berdasarkan department

```sql
SELECT employee_id,
       first_name,
       last_name,
       salary
FROM employees
WHERE department_id = :department_id;
```

Jika APEX/SQL environment tidak mendukung bind variable pada soal, ganti langsung:

```sql
WHERE department_id = 80;
```

## 53.2 Cari employee dengan rentang salary

```sql
SELECT employee_id,
       last_name,
       salary
FROM employees
WHERE salary BETWEEN :min_salary AND :max_salary
ORDER BY salary DESC;
```

## 53.3 Cari nama dengan pola

```sql
SELECT employee_id,
       first_name,
       last_name
FROM employees
WHERE UPPER(last_name) LIKE UPPER('%:keyword%');
```

Jika nilai langsung:

```sql
WHERE UPPER(last_name) LIKE '%SON%';
```

## 53.4 Employee dan nama department

```sql
SELECT e.employee_id,
       e.first_name,
       e.last_name,
       d.department_name
FROM employees e
JOIN departments d
  ON e.department_id = d.department_id;
```

## 53.5 Employee, job title, department, city

```sql
SELECT e.employee_id,
       e.first_name,
       e.last_name,
       j.job_title,
       d.department_name,
       l.city
FROM employees e
JOIN jobs j
  ON e.job_id = j.job_id
JOIN departments d
  ON e.department_id = d.department_id
JOIN locations l
  ON d.location_id = l.location_id;
```

## 53.6 Employee dan manager

```sql
SELECT e.last_name AS employee,
       m.last_name AS manager
FROM employees e
LEFT JOIN employees m
  ON e.manager_id = m.employee_id;
```

## 53.7 Total dan rata-rata salary per department

```sql
SELECT department_id,
       COUNT(*) AS employee_count,
       SUM(salary) AS total_salary,
       ROUND(AVG(salary), 2) AS avg_salary,
       MIN(salary) AS min_salary,
       MAX(salary) AS max_salary
FROM employees
GROUP BY department_id
ORDER BY department_id;
```

## 53.8 Department dengan rata-rata di atas nilai tertentu

```sql
SELECT department_id,
       ROUND(AVG(salary), 2) AS avg_salary
FROM employees
GROUP BY department_id
HAVING AVG(salary) > :minimum_average
ORDER BY avg_salary DESC;
```

## 53.9 Employee di atas rata-rata perusahaan

```sql
SELECT employee_id,
       last_name,
       salary
FROM employees
WHERE salary > (
    SELECT AVG(salary)
    FROM employees
)
ORDER BY salary DESC;
```

## 53.10 Employee di atas rata-rata department masing-masing

```sql
SELECT e.employee_id,
       e.last_name,
       e.salary,
       e.department_id
FROM employees e
WHERE e.salary > (
    SELECT AVG(e2.salary)
    FROM employees e2
    WHERE e2.department_id = e.department_id
)
ORDER BY e.department_id, e.salary DESC;
```

## 53.11 Department yang tidak memiliki employee

```sql
SELECT d.department_id,
       d.department_name
FROM departments d
WHERE NOT EXISTS (
    SELECT 1
    FROM employees e
    WHERE e.department_id = d.department_id
);
```

## 53.12 Top 5 salary

```sql
SELECT *
FROM (
    SELECT employee_id,
           last_name,
           salary
    FROM employees
    ORDER BY salary DESC
)
WHERE ROWNUM <= 5;
```

## 53.13 Employee yang direkrut pada tahun tertentu

```sql
SELECT employee_id,
       last_name,
       hire_date
FROM employees
WHERE TO_CHAR(hire_date, 'YYYY') = '2000';
```

Alternatif yang lebih baik untuk rentang tanggal:

```sql
SELECT employee_id,
       last_name,
       hire_date
FROM employees
WHERE hire_date >= TO_DATE('01-01-2000', 'DD-MM-YYYY')
  AND hire_date <  TO_DATE('01-01-2001', 'DD-MM-YYYY');
```

## 53.14 Hitung masa kerja

```sql
SELECT last_name,
       hire_date,
       ROUND(MONTHS_BETWEEN(SYSDATE, hire_date) / 12, 2) AS years_worked
FROM employees;
```

## 53.15 Klasifikasi salary

```sql
SELECT last_name,
       salary,
       CASE
           WHEN salary >= 10000 THEN 'HIGH'
           WHEN salary >= 5000  THEN 'MEDIUM'
           ELSE 'LOW'
       END AS salary_category
FROM employees;
```

## 53.16 Salary termasuk grade mana

```sql
SELECT e.last_name,
       e.salary,
       g.grade_level
FROM employees e
JOIN job_grades g
  ON e.salary BETWEEN g.lowest_sal AND g.highest_sal;
```

## 53.17 Gabungkan data current employee dan job history

```sql
SELECT employee_id, job_id
FROM employees
UNION
SELECT employee_id, job_id
FROM job_history;
```

## 53.18 Employee yang pernah ada di job history

```sql
SELECT employee_id
FROM employees
INTERSECT
SELECT employee_id
FROM job_history;
```

## 53.19 Employee current yang tidak ada di job history

```sql
SELECT employee_id
FROM employees
MINUS
SELECT employee_id
FROM job_history;
```

## 53.20 INSERT lalu COMMIT

```sql
INSERT INTO departments
       (department_id, department_name, location_id)
VALUES (280, 'Data Science', 1700);

COMMIT;
```

## 53.21 UPDATE aman

```sql
SELECT employee_id, last_name, salary
FROM employees
WHERE department_id = 80;

UPDATE employees
SET salary = salary * 1.05
WHERE department_id = 80;

COMMIT;
```

Untuk ujian, pola `SELECT dulu -> pastikan target -> UPDATE/DELETE` membantu menghindari salah kondisi.

## 53.22 DELETE aman

```sql
SELECT employee_id, last_name
FROM employees
WHERE employee_id = 300;

DELETE FROM employees
WHERE employee_id = 300;

COMMIT;
```

---

# 54. Perbedaan yang Sering Tertukar

## WHERE vs HAVING

```sql
-- Filter baris
WHERE salary > 5000
```

```sql
-- Filter kelompok
HAVING AVG(salary) > 5000
```

## DELETE vs TRUNCATE vs DROP

```text
DELETE   -> menghapus baris, dapat memakai WHERE, DML
TRUNCATE -> menghapus semua baris dengan cepat, DDL
DROP     -> menghapus objek tabel beserta strukturnya
```

```sql
DELETE FROM employees
WHERE department_id = 80;
```

```sql
TRUNCATE TABLE emp_copy;
```

```sql
DROP TABLE emp_copy;
```

## UNION vs UNION ALL

```text
UNION     -> duplikat dihapus
UNION ALL -> duplikat dipertahankan
```

## INNER JOIN vs OUTER JOIN

```text
INNER -> hanya yang match
LEFT  -> semua kiri + match kanan
RIGHT -> semua kanan + match kiri
FULL  -> semua kedua sisi
```

## COUNT(*) vs COUNT(column)

```text
COUNT(*)      -> menghitung semua baris
COUNT(column) -> hanya nilai non-NULL
```

## `=` vs `IN` pada subquery

```text
=  -> subquery harus menghasilkan satu nilai
IN -> subquery boleh menghasilkan banyak nilai
```

## ANY vs ALL

```text
> ANY -> lebih besar dari minimal satu nilai
> ALL -> lebih besar dari semua nilai
< ANY -> lebih kecil dari minimal satu nilai
< ALL -> lebih kecil dari semua nilai
```

## NVL vs COALESCE

```text
NVL(a,b)              -> jika a NULL, gunakan b
COALESCE(a,b,c,...)   -> nilai non-NULL pertama
```

## PRIMARY KEY vs UNIQUE

```text
PRIMARY KEY -> unik dan tidak boleh NULL; satu PK per tabel
UNIQUE      -> menjaga keunikan; dapat ada beberapa UNIQUE constraint
```

## VARCHAR2 vs CHAR

```text
VARCHAR2(n) -> panjang aktual bervariasi
CHAR(n)     -> panjang tetap
```

---

# 55. Kesalahan SQL yang Sangat Sering Terjadi

## 55.1 Lupa tanda petik teks

Salah:

```sql
WHERE last_name = Taylor
```

Benar:

```sql
WHERE last_name = 'Taylor'
```

## 55.2 Membandingkan NULL dengan `=`

Salah:

```sql
WHERE commission_pct = NULL
```

Benar:

```sql
WHERE commission_pct IS NULL
```

## 55.3 Fungsi agregat di WHERE

Salah:

```sql
SELECT department_id, AVG(salary)
FROM employees
WHERE AVG(salary) > 5000
GROUP BY department_id;
```

Benar:

```sql
SELECT department_id, AVG(salary)
FROM employees
GROUP BY department_id
HAVING AVG(salary) > 5000;
```

## 55.4 Kolom non-agregat tidak dimasukkan GROUP BY

Salah:

```sql
SELECT department_id,
       job_id,
       AVG(salary)
FROM employees
GROUP BY department_id;
```

Benar:

```sql
SELECT department_id,
       job_id,
       AVG(salary)
FROM employees
GROUP BY department_id, job_id;
```

## 55.5 Subquery multi-row memakai `=`

Berisiko salah:

```sql
WHERE department_id = (
    SELECT department_id
    FROM departments
    WHERE location_id = 1700
)
```

Jika hasil bisa banyak:

```sql
WHERE department_id IN (
    SELECT department_id
    FROM departments
    WHERE location_id = 1700
)
```

## 55.6 Lupa kondisi JOIN

Salah:

```sql
FROM employees e, departments d
```

Tanpa kondisi akan menjadi Cartesian product.

Benar:

```sql
FROM employees e
JOIN departments d
  ON e.department_id = d.department_id
```

## 55.7 AND/OR tanpa tanda kurung

Jika logika kompleks:

```sql
WHERE (department_id = 50 OR department_id = 80)
  AND salary > 5000
```

lebih aman daripada mengandalkan precedence tanpa memahami maksud soal.

## 55.8 UPDATE/DELETE tanpa WHERE

```sql
UPDATE employees
SET salary = 0;
```

Mengubah semua baris.

```sql
DELETE FROM employees;
```

Menghapus semua baris.

Selalu periksa apakah soal memang meminta seluruh tabel.

## 55.9 NOT IN dengan NULL

Query seperti:

```sql
WHERE employee_id NOT IN (
    SELECT manager_id
    FROM employees
)
```

bisa memberi hasil yang tidak diharapkan jika `manager_id` mengandung `NULL`.

Alternatif aman:

```sql
WHERE NOT EXISTS (
    SELECT 1
    FROM employees m
    WHERE m.manager_id = e.employee_id
)
```

atau filter NULL dalam subquery jika logikanya memang tepat.

---

# 56. Urutan Memecahkan Soal Studi Kasus

Contoh soal:

> Tampilkan nama lengkap, nama department, job title, dan salary employee pada department 80 yang salary-nya lebih besar dari rata-rata seluruh employee. Urutkan dari salary tertinggi.

Pecah:

```text
nama lengkap           -> first_name || ' ' || last_name
nama department        -> departments
job title              -> jobs
salary                 -> employees.salary
department 80          -> WHERE e.department_id = 80
lebih dari rata-rata   -> subquery AVG(salary)
urut tertinggi         -> ORDER BY e.salary DESC
```

Query:

```sql
SELECT e.first_name || ' ' || e.last_name AS full_name,
       d.department_name,
       j.job_title,
       e.salary
FROM employees e
JOIN departments d
  ON e.department_id = d.department_id
JOIN jobs j
  ON e.job_id = j.job_id
WHERE e.department_id = 80
  AND e.salary > (
      SELECT AVG(salary)
      FROM employees
  )
ORDER BY e.salary DESC;
```

Contoh soal:

> Tampilkan department yang memiliki lebih dari 5 employee dan rata-rata salary di atas 6000.

Pecah:

```text
per department             -> GROUP BY department_id
jumlah employee            -> COUNT(*)
lebih dari 5                -> HAVING COUNT(*) > 5
rata-rata salary            -> AVG(salary)
rata-rata > 6000            -> HAVING AVG(salary) > 6000
```

Query:

```sql
SELECT department_id,
       COUNT(*) AS employee_count,
       ROUND(AVG(salary), 2) AS avg_salary
FROM employees
GROUP BY department_id
HAVING COUNT(*) > 5
   AND AVG(salary) > 6000;
```

Contoh soal:

> Tampilkan semua department termasuk department yang belum memiliki employee.

```sql
SELECT d.department_id,
       d.department_name,
       e.employee_id,
       e.last_name
FROM departments d
LEFT JOIN employees e
  ON d.department_id = e.department_id
ORDER BY d.department_id;
```

---

# 57. Template Kosong Universal

## SELECT satu tabel

```sql
SELECT kolom_1,
       kolom_2
FROM tabel
WHERE kondisi
ORDER BY kolom_1;
```

## JOIN dua tabel

```sql
SELECT a.kolom,
       b.kolom
FROM tabel_a a
JOIN tabel_b b
  ON a.foreign_key = b.primary_key
WHERE kondisi;
```

## JOIN tiga tabel

```sql
SELECT a.kolom,
       b.kolom,
       c.kolom
FROM tabel_a a
JOIN tabel_b b
  ON a.key_b = b.key_b
JOIN tabel_c c
  ON b.key_c = c.key_c
WHERE kondisi;
```

## GROUP BY

```sql
SELECT kolom_kelompok,
       COUNT(*) AS jumlah,
       AVG(kolom_angka) AS rata_rata
FROM tabel
WHERE kondisi_baris
GROUP BY kolom_kelompok
HAVING kondisi_agregat
ORDER BY kolom_kelompok;
```

## Subquery single-row

```sql
SELECT ...
FROM ...
WHERE kolom > (
    SELECT AVG(kolom)
    FROM ...
);
```

## Subquery multi-row

```sql
SELECT ...
FROM ...
WHERE kolom IN (
    SELECT kolom
    FROM ...
    WHERE ...
);
```

## Correlated subquery

```sql
SELECT ...
FROM tabel_luar a
WHERE a.nilai > (
    SELECT AVG(b.nilai)
    FROM tabel_luar b
    WHERE b.group_id = a.group_id
);
```

## EXISTS

```sql
SELECT ...
FROM parent p
WHERE EXISTS (
    SELECT 1
    FROM child c
    WHERE c.parent_id = p.parent_id
);
```

## INSERT

```sql
INSERT INTO tabel (kolom_1, kolom_2)
VALUES (nilai_1, nilai_2);
```

## UPDATE

```sql
UPDATE tabel
SET kolom = nilai
WHERE kondisi;
```

## DELETE

```sql
DELETE FROM tabel
WHERE kondisi;
```

## CREATE TABLE

```sql
CREATE TABLE tabel (
    id   NUMBER PRIMARY KEY,
    nama VARCHAR2(100) NOT NULL
);
```

## ALTER TABLE

```sql
ALTER TABLE tabel
ADD kolom_baru VARCHAR2(50);
```

## CREATE VIEW

```sql
CREATE OR REPLACE VIEW nama_view AS
SELECT ...
FROM ...
WHERE ...;
```

---

# 58. Ringkasan Last-Minute

```text
SELECT        pilih kolom
FROM          sumber data
WHERE         filter baris
GROUP BY      bentuk kelompok
HAVING        filter kelompok
ORDER BY      urutkan hasil

DISTINCT      hilangkan duplikat
||            gabungkan teks
AS            alias
BETWEEN       rentang inklusif
IN            beberapa pilihan
LIKE          pola
%             0 atau banyak karakter
_             tepat 1 karakter
IS NULL       cek NULL

AND           kedua kondisi benar
OR            salah satu benar
NOT           negasi

UPPER/LOWER/INITCAP
SUBSTR/LENGTH/INSTR
LPAD/RPAD/TRIM/REPLACE
ROUND/TRUNC/MOD
SYSDATE/MONTHS_BETWEEN/ADD_MONTHS/NEXT_DAY/LAST_DAY
TO_CHAR/TO_DATE/TO_NUMBER
NVL/NVL2/NULLIF/COALESCE
CASE/DECODE

JOIN ON       kondisi join eksplisit
JOIN USING    kolom join nama sama
NATURAL JOIN  otomatis berdasarkan nama kolom sama
LEFT JOIN     semua kiri
RIGHT JOIN    semua kanan
FULL JOIN     semua kedua sisi
SELF JOIN     tabel ke dirinya sendiri

SUM/AVG/COUNT/MIN/MAX/STDDEV/VARIANCE
GROUP BY/HAVING
ROLLUP/CUBE/GROUPING SETS

UNION         gabung tanpa duplikat
UNION ALL     gabung dengan duplikat
INTERSECT     irisan
MINUS         selisih

Subquery 1 baris -> =, >, <, >=, <=, <>
Subquery banyak  -> IN, ANY, ALL
EXISTS           cek keberadaan
NOT EXISTS       cek ketidakadaan
WITH             named subquery

INSERT           tambah baris
UPDATE           ubah baris
DELETE           hapus baris
COMMIT           simpan transaksi
ROLLBACK         batalkan transaksi

CREATE TABLE     buat tabel
ALTER TABLE      ubah struktur
TRUNCATE TABLE   kosongkan isi
DROP TABLE       hapus objek

NOT NULL / UNIQUE / PRIMARY KEY / FOREIGN KEY / CHECK

CREATE VIEW
WITH CHECK OPTION
WITH READ ONLY
DROP VIEW

SEQUENCE.NEXTVAL
SEQUENCE.CURRVAL
CREATE INDEX
CREATE SYNONYM
```

---

# 59. Checklist Sebelum Menekan Run

```text
[ ] Nama tabel benar
[ ] Nama kolom benar
[ ] Alias tabel konsisten
[ ] String memakai 'single quote'
[ ] NULL memakai IS NULL / IS NOT NULL
[ ] Join punya kondisi ON/USING
[ ] Tidak ada Cartesian product tidak sengaja
[ ] AND/OR diberi tanda kurung jika perlu
[ ] GROUP BY memuat semua kolom non-agregat yang diperlukan
[ ] HAVING digunakan untuk aggregate condition
[ ] Subquery single-row tidak menghasilkan banyak baris
[ ] IN/ANY/ALL dipakai bila subquery multi-row
[ ] UPDATE/DELETE memiliki WHERE jika tidak ingin semua baris
[ ] ORDER BY sesuai permintaan soal
[ ] Semicolon ada di akhir statement
```

---

# 60. Indeks Modul Sumber EP 01–50

| EP | Bagian | Materi |
|---:|---|---|
| 01 | 1-1 | Oracle APEX |
| 02 | 1-2 | Teknologi Relational Database |
| 03 | 1-3 | Anatomi Pernyataan SQL |
| 04 | 2-1 | Kolom, Karakter, dan Baris |
| 05 | 2-2 | Membatasi Baris yang Dipilih |
| 06 | 2-3 | Operator Perbandingan |
| 07 | 3-1 | Perbandingan Logika dan Aturan Presedensi |
| 08 | 3-2 | Mengurutkan Baris |
| 09 | 3-3 | Pengantar Fungsi |
| 10 | 4-1 | Manipulasi Huruf dan Karakter |
| 11 | 4-2 | Fungsi Angka |
| 12 | 4-3 | Fungsi Tanggal |
| 13 | 5-1 | Fungsi Konversi |
| 14 | 5-2 | Fungsi NULL |
| 15 | - | Berkas EP 15 tidak tersedia pada unggahan |
| 16 | 5-3 | Ekspresi Kondisional |
| 17 | 5-2 | Fungsi NULL (materi berulang) |
| 18 | 5-3 | Ekspresi Kondisional (materi berulang) |
| 19 | 6-1 | Cross Join dan Natural Join |
| 20 | 6-2 | Klausa Join |
| 21 | 6-3 | Inner Join versus Outer Join |
| 22 | 6-4 | Self-Join dan Kueri Hierarkis |
| 23 | 7-1 | Oracle Equijoin dan Produk Kartesius |
| 24 | 7-2 | Oracle Nonequijoin dan Outer Join |
| 25 | 8-1 | Fungsi Grup |
| 26 | 8-2 | COUNT, DISTINCT, NVL |
| 27 | 8-1 | Fungsi Grup (materi berulang) |
| 28 | 8-2 | COUNT, DISTINCT, NVL (materi berulang) |
| 29 | 9-1 | GROUP BY dan HAVING |
| 30 | 9-2 | ROLLUP, CUBE, GROUPING SETS |
| 31 | 9-3 | Operator Set |
| 32 | 10-1 | Dasar-Dasar Subkueri |
| 33 | 10-2 | Subkueri Baris Tunggal |
| 34 | 10-3 | Subkueri Multi-Baris |
| 35 | 10-4 | Subkueri Terkorelasi |
| 36 | 11-1 | Memastikan Kualitas Hasil Kueri |
| 37 | 12-1 | Pernyataan INSERT |
| 38 | 12-2 | Memperbarui Nilai Kolom dan Menghapus Baris |
| 39 | 12-3 | Nilai DEFAULT, MERGE, dan INSERT Multi-Tabel |
| 40 | 13-1 | Membuat Tabel |
| 41 | 13-2 | Menggunakan Tipe Data |
| 42 | 13-3 | Memodifikasi Tabel |
| 43 | 14-1 | Constraint NOT NULL dan UNIQUE |
| 44 | 14-2 | PRIMARY KEY, FOREIGN KEY, CHECK |
| 45 | 14-3 | Mengelola Constraint |
| 46 | 15-1 | Membuat View |
| 47 | 15-2 | Operasi DML dan View |
| 48 | 15-3 | Mengelola View |
| 49 | 16-1 | Bekerja dengan Sequence |
| 50 | 16-2 | Index dan Synonym |

---

## Penutup

Untuk soal studi kasus, jangan mulai dengan menghafal satu query panjang. Pecah permintaan menjadi komponen: kolom yang ditampilkan, tabel, hubungan antar tabel, filter, agregasi, subquery, dan pengurutan. Setelah itu susun klausa SQL sesuai urutannya dan cek kembali hasil yang diharapkan.
