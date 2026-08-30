Character functions digunakan untuk memproses **data berbentuk karakter/string**.

---

## UPPER()

Mengubah seluruh karakter menjadi huruf besar.

```sql
SELECT UPPER('oracle sql')
FROM dual;
```

Hasil:

```text
ORACLE SQL
```

Contoh dengan tabel:

```sql
SELECT UPPER(nama) AS nama
FROM mahasiswa;
```

---

## LOWER()

Mengubah seluruh karakter menjadi huruf kecil.

```sql
SELECT LOWER('ORACLE SQL')
FROM dual;
```

Hasil:

```text
oracle sql
```

---

## INITCAP()

Mengubah huruf pertama setiap kata menjadi uppercase.

```sql
SELECT INITCAP('oracle sql database')
FROM dual;
```

Hasil:

```text
Oracle Sql Database
```

---

## LENGTH()

Menghitung jumlah karakter.

```sql
SELECT LENGTH('Oracle')
FROM dual;
```

Hasil:

```text
6
```

Contoh:

```sql
SELECT nama,
       LENGTH(nama) AS panjang_nama
FROM mahasiswa;
```

---

## SUBSTR()

Mengambil sebagian karakter dari sebuah string.

Syntax:

```text
SUBSTR(string, posisi_awal, jumlah_karakter)
```

Contoh:

```sql
SELECT SUBSTR('ORACLE', 1, 3)
FROM dual;
```

Hasil:

```text
ORA
```

Oracle menggunakan posisi karakter yang dimulai dari **1**.

---

## INSTR()

Mencari posisi kemunculan suatu karakter atau substring.

```sql
SELECT INSTR('ORACLE SQL', 'SQL')
FROM dual;
```

Hasil menunjukkan posisi awal `SQL`.

---

## REPLACE()

Mengganti substring tertentu dengan substring lainnya.

```sql
SELECT REPLACE(
    'Oracle Database',
    'Database',
    'SQL'
)
FROM dual;
```

Hasil:

```text
Oracle SQL
```

---

## TRIM()

Menghapus karakter tertentu dari awal dan/atau akhir string. Penggunaan paling umum adalah menghapus spasi.

```sql
SELECT TRIM('   Oracle SQL   ')
FROM dual;
```

Hasil:

```text
Oracle SQL
```

---

## LTRIM()

Menghapus karakter/spasi dari sisi kiri.

```sql
SELECT LTRIM('   Oracle')
FROM dual;
```

---

## RTRIM()

Menghapus karakter/spasi dari sisi kanan.

```sql
SELECT RTRIM('Oracle   ')
FROM dual;
```

---
